# Как обновить сертификаты с истекшим сроком действия

Подключаемся к HOSTVM Node с истёкшим сертификатом (в примере имя узла hostvm-test).

Создаем запрос сертификата на основании ключа службы VDSM `/etc/pki/vdsm/keys/vdsmkey.pem`:

```
[root@hostvm-test ~]# openssl req -new \
-key /etc/pki/vdsm/keys/vdsmkey.pem \
-out /tmp/test_hostvm-test_vdsm.csr \
-batch \
-subj "/"
```

Подписываем запрос с помощью корневого сертификата управляющей машины, для этого подключаемся через SSH к ВМ с HOSTVM Manager, копируем запрос с HOSTVM Node (hostvm-test) на HOSTVM Manager в /tmp и выполняем команды:

```
[root@engine-test ~]# cd /etc/pki/ovirt-engine/
[root@engine-test ~]# openssl ca -batch \
-policy policy_match \
-config openssl.conf \
-cert ca.pem \
-keyfile private/ca.pem \
-days +"365" \
-in /tmp/test_hostvm-test_vdsm.csr \
-out /tmp/test_hostvm-test_vdsm.cer \
-startdate `(date --utc --date "now -1 days" +"%y%m%d%H%M%SZ")` \
-subj "/O=example.test/CN=hostvm-test.example.test\
-utf8
```

Имя субъекта в сертификате должно быть в формате `"/O=example.test/CN=hostvm-test.example.test"`, укажите ваши значения.

Копируем новый сертификат с HOSTVM Manager на HOSTVM Node(hostvm-test) в /tmp.

Копируем новый сертификат в каталоги служб, предварительно создав копию существующих сертификатов:

```
[root@hostvm-test ~]# cp /etc/pki/vdsm/certs/vdsmcert.pem /etc/pki/vdsm/certs/vdsmcert.pem.bak
[root@hostvm-test ~]# cp /tmp/test_hostvm-test_vdsm.cer /etc/pki/vdsm/certs/vdsmcert.pem
[root@hostvm-test ~]# cp /etc/pki/vdsm/libvirt-spice/server-cert.pem /etc/pki/vdsm/libvirt-spice/server-cert.pem.bak
[root@hostvm-test ~]# cp /etc/pki/vdsm/certs/vdsmcert.pem /etc/pki/vdsm/libvirt-spice/server-cert.pem
[root@hostvm-test ~]# cp /etc/pki/libvirt/clientcert.pem /etc/pki/libvirt/clientcert.pem.bak
[root@hostvm-test ~]# cp /etc/pki/vdsm/certs/vdsmcert.pem /etc/pki/libvirt/clientcert.pem
```

Перезапускаем службы:

```
[root@hostvm-test ~]# systemctl restart libvirtd
[root@hostvm-test ~]# systemctl restart vdsmd
```

Проверяем срок действия сертификата

```
[root@hostvm-test ~]# openssl x509 -noout -enddate -in /etc/pki/vdsm/certs/vdsmcert.pem
```

Повторяем процедуру на остальных узлах с истёкшим сроком действия сертификатов. У созданных таким образом сертификатов будет отсутствовать subject alternative name, о чём может быть выдано предупреждение: Certificate of host hostvm-test.example.test is invalid. The certificate doesn't contain valid subject alternative name, please enroll new certificate for the host. Поэтому после восстановления доступа к web-интерфейсу необходимо выполнить обновление сертификатов согласно [инструкции](kak-peregenerirovat-ssl-sertifikaty-na-hosted-engine-i-khostakh.md)

Выполняем обновление сертификатов управляющей машины, как описано в [статье](kak-peregenerirovat-ssl-sertifikaty-na-hosted-engine-i-khostakh.md)\
