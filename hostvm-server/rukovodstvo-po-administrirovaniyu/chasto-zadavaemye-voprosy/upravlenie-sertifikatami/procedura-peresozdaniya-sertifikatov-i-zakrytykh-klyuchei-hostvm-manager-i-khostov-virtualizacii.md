---
description: >-
  Инструкция описывает процедуру замены внутреннего центра сертификации HOSTVM
  Manager, его закрытого ключа, а также сертификатов и закрытых ключей хостов
  виртуализации.
---

# Процедура пересоздания сертификатов и закрытых ключей HOSTVM Manager и хостов виртуализации

{% hint style="warning" %}
Перед выполнением процедуры обязательно протестируйте все действия на тестовом стенде.
{% endhint %}

### Подготовка и резервное копирование

{% hint style="warning" %}
При выполнении процедуры все ранее выданные сертификаты становятся недействительными. До завершения повторной регистрации сертификатов на всех хостах управление хостами и виртуальными машинами через HOSTVM Manager будет недоступно.<br>

В связи с этим на время выполнения процедуры необходимо соблюдение следующих условий:

* не выполняйте операции с хостами и виртуальными машинами через портал управления;&#x20;
* заранее согласуйте технологическое окно;
* убедитесь в наличии доступа по SSH и консоли к HOSTVM Manager и всем хостам;
* проверьте наличие свободного места для резервных копий;
* не удаляйте исходные сертификаты и закрытые ключи до завершения проверки.
{% endhint %}

1. Отключите управление питанием Fencing.

* Откройте настройки каждого кластера.
* Перейдите в раздел Fencing policy.
* Уберите флажок Enable fencing.
* Сохраните изменения.

2. Если управляющая машина развёрнута в режиме Self-Hosted, подключитесь по SSH к хосту, на котором запущена ВМ HOSTVM Manager, переведите кластер в режим обслуживания и убедитесь, что он перешёл в режим global maintenance:

```
hosted-engine --set-maintenance --mode=global
hosted-engine --vm-status
```

{% hint style="warning" %}
Если управляющая машина развёрнута в режиме Standalone, при обновлении сертификатов на HOSTVM Manager переводить гипервизор в режим обслуживания не нужно.
{% endhint %}

4. Подключитесь по SSH к ВМ HOSTVM Manager и остановите службу:

```
systemctl stop ovirt-engine
systemctl disable ovirt-engine
```

5. Создайте резервную копию управляющей машины:

```
engine-backup --scope=all --mode=backup --file=/root/hvm-backup.tgz --log=/root/backup.log
```

6. Создайте резервную копию сертификатов управляющей машины:

```
tar cJpf /root/pki.tar.xz /etc/pki
```

7. Скопируйте архивы `/root/hvm-backup.tgz` и `/root/``pki.tar.xz` на внешнее хранилище.
8. Запустите службы:

```
systemctl start ovirt-engine
systemctl enable ovirt-engine
```

9. Убедитесь, что служба запущена:

```
systemctl status ovirt-engine --no-pager
```

10. Создайте резервную копию сертификатов на каждом хосте:

```
tar -cJpf /root/pki-host-$(hostname -s).tar.xz /etc/pki
```

11. Скопируйте архивы со всех хостов на внешнее хранилище.

### Создание нового центра сертификации

{% hint style="info" %}
В примерах используются следующие значения:

```
HOSTVM Manager: engine.hostvm.test
Хост: node1.hostvm.test
IP-адрес хоста: 10.1.99.32
Организация: hostvm.test
```

Замените эти значения на параметры конкретной среды.
{% endhint %}

1. Извлеките субъект существующего сертификата:

```
SUBJECT="$(openssl x509 -in /etc/pki/ovirt-engine/ca.pem -subject -noout -nameopt compat | sed 's;subject=\(.*\);\1;')"
```

2. Проверьте корректность и соответствие извлеченного субъекта:

```
echo "${SUBJECT}"
```

3. Создайте новый центр сертификации

```
/usr/share/ovirt-engine/bin/pki-create-ca.sh --subject="${SUBJECT}" --keystore-password=mypass --ca-file=ca
```

{% hint style="warning" %}
Параметр `--password=mypass` должна быть быть указан именно так, как в примере. Не заменяйте `mypass` на другой пароль
{% endhint %}

4.  Перед запуском `engine-setup` рекомендуется ознакомиться с ответами, сохранёнными во время предыдущей настройки. Это позволяет восстановить использованные ранее параметры, проверить текущую конфигурацию и при необходимости повторно применить её.

    Файлы ответов расположены в каталоге:

    ```
    /var/lib/ovirt-engine/setup/answers/
    ```

    Каждый файл содержит параметры соответствующего запуска `engine-setup`, включая значения, указанные как при первоначальной установке, так и при последующей перенастройке управляющей машины.
5. Запустите процесс настройки управляющей машины в оффлайн режиме и ответьте на вопросы установщика:

```
engine-setup --offline
```

6. После завершения убедитесь, что служба Manager запущена:

```
systemctl status ovirt-engine --no-pager
```

### Повторная регистрация сертификата хоста

{% hint style="warning" %}
Повторите этот раздел для каждого хоста виртуализации.
{% endhint %}

1. На хосте выполните:

```
openssl x509 -in /etc/pki/vdsm/certs/vdsmcert.pem -subject -noout
```

Определите, используется ли в сертификате полное доменное имя или IP-адрес. В дальнейшем значение CN и параметр SAN должны соответствовать фактическому имени или адресу хоста.

2. На хосте создайте временный файл с конфигурацией openssl /tmp/node1.hostvm.test.conf.

```
cat > /tmp/node1.hostvm.test.conf <<'EOF'
RANDFILE = ${ENV::HOME}/.rnd

[ req ]
distinguished_name = req_distinguished_name
prompt = no

[ v3_ca ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer:always
basicConstraints = CA:true

[ req_distinguished_name ]
O = hostvm.test
CN = node1.hostvm.test
EOF
```

Замените организацию (O) и общее название (CN) на значения полученные на шаге 1:

3. Создайте новый закрытый ключ и запрос на подпись:

```
openssl req -new -newkey rsa:2048 -nodes -keyout /etc/pki/vdsm/keys/vdsmkey.pem -out /tmp/node1.hostvm.test.req -config /tmp/node1.hostvm.test.conf
```

4. Проверьте созданные файлы:

```
ls -l \
  /etc/pki/vdsm/keys/vdsmkey.pem \
  /tmp/node1.hostvm.test.req
```

5. Скопируйте закрытый ключ в каталоги, используемые libvirt::

```
cp -p /etc/pki/vdsm/keys/vdsmkey.pem /etc/pki/vdsm/libvirt-spice/server-key.pem
cp -p /etc/pki/vdsm/keys/vdsmkey.pem /etc/pki/vdsm/libvirt-vnc/server-key.pem
cp -p /etc/pki/vdsm/keys/vdsmkey.pem /etc/pki/libvirt/private/clientkey.pem
```

Если `cp` запрашивает подтверждение перезаписи, проверьте пути и подтвердите операцию только после того, как убедитесь, что используется правильный хост.

6. Скопируйте CSR на HOSTVM Manager:

```
scp /tmp/node1.hostvm.test.req root@engine.hostvm.test:/etc/pki/ovirt-engine/requests/
```

7. На HOSTVM Manager измените владельца файла:

```
chown ovirt:ovirt /etc/pki/ovirt-engine/requests/node1.hostvm.test.req
```

8. Подпишите сертификат в зависимости от способа использования CN:

* Если в CN используется полное доменное имя:

```
/usr/share/ovirt-engine/bin/pki-enroll-request.sh --name=node1.hostvm.test --subject="/O=hostvm.test/CN=node1.hostvm.test" --san="DNS:node1.hostvm.test" --days=3650
```

Параметр `--name` должен совпадать с именем файла CSR без расширения `.req`

* Если в CN используется IP-адрес:

```
/usr/share/ovirt-engine/bin/pki-enroll-request.sh --name=10.1.99.32 --subject="/O=example.com/CN=10.1.99.32" --san="IP:10.1.99.32" --days=3650
```

9. Проверьте созданный сертификат:

```
openssl x509 \
  -in /etc/pki/ovirt-engine/certs/node1.hostvm.test.cer \
  -noout \
  -subject \
  -issuer \
  -dates \
  -ext subjectAltName
```

Для IP-варианта замените имя файла на соответствующее значение.

10. Скопируйте ЦС и подписанный сертификат обратно на хост:

```
HOST="node1.hostvm.test"
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/ca.pem root@${HOST}:/etc/pki/CA/cacert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/ca.pem root@${HOST}:/etc/pki/vdsm/certs/cacert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/ca.pem root@${HOST}:/etc/pki/vdsm/libvirt-spice/ca-cert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/ca.pem root@${HOST}:/etc/pki/vdsm/libvirt-vnc/ca-cert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/certs/${HOST}.cer root@${HOST}:/etc/pki/vdsm/certs/vdsmcert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/certs/${HOST}.cer root@${HOST}:/etc/pki/vdsm/libvirt-spice/server-cert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/certs/${HOST}.cer root@${HOST}:/etc/pki/vdsm/libvirt-vnc/server-cert.pem
scp -i /etc/pki/ovirt-engine/keys/engine_id_rsa /etc/pki/ovirt-engine/certs/${HOST}.cer root@${HOST}:/etc/pki/libvirt/clientcert.pem
```

При запросе пароля от хоста необходимо его ввести.

11. На хосте проверьте сертификат:

```
openssl x509 \
  -in /etc/pki/vdsm/certs/vdsmcert.pem \
  -noout \
  -subject \
  -issuer \
  -dates \
  -ext subjectAltName
```

12. Перезапустите службы на хосте, чтобы обновить сертификаты:

```
systemctl restart libvirtd
systemctl restart mom-vdsm
systemctl restart ovirt-imageio
systemctl restart vdsmd
systemctl restart supervdsmd
```

13. Проверьте их состояние:

```
systemctl --failed
systemctl status libvirtd mom-vdsm ovirt-imageio vdsmd supervdsmd --no-pager
```

### Обновление сертификатов виртуальных машин

После обновления сертификатов хостов уже запущенные виртуальные машины могут продолжать использовать старые процессы QEMU и связанные с ними сертификаты консольного доступа.

Для применения новых сертификатов выполните одно из действий:

* выполните динамическую миграцию виртуальной машины на другой хост;
* корректно выключите виртуальную машину и снова запустите её.

Перед выполнением операции проверьте доступность целевого хоста и наличие достаточного количества ресурсов.

### Завершение процедуры&#x20;

После успешной повторной регистрации всех хостов, включите управление питанием:

1. Откройте настройки каждого кластера.
2. Перейдите в раздел Fencing policy.
3. Установите флажок Enable fencing.
4. Сохраните изменения.

Если управляющая машина развернута в режиме Self-Hosted выполните отключение режима обслуживания. Для этого на хосте, на котором запущена ВМ HOSTVM Manager и выполните:

```
hosted-engine --set-maintenance --mode=none
```

Убедитесь, что кластер вышел из режима global maintenance:

```
hosted-engine --vm-status
```
