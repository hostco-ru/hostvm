---
description: >-
  Данное руководство описывает процесс обновления сертификатов для сервиса VNC,
  на гипервизорах HOSTVM версии 4.4.* и ранее, используемого виртуальными
  машинами для удаленного доступа и управления.
---

# Обновление сертификата VNC на гипервизорах HOSTVM

В HOSTVM виртуализация версий 4.4.\* и ранее требуется ручное обновление сертификата&#x20;

```
/etc/pki/vdsm/libvirt-vnc/server-cert.pem
```

{% hint style="warning" %}
По истечении срока действия этого сертификата все виртуальные машины, использующие протокол VNC в кластерах с включенным шифрованием VNC, перестанут запускаться с ошибкой:

\# VM example.com is down with error. Exit message: internal error: process exited while connecting to monitor: 2025-09-05T04:35:55.651530Z qemu-kvm:  The server certificate /etc/pki/vdsm/libvirt-vnc/server-cert.pem has expired.
{% endhint %}

Для обновления сертификата VNC выполните следующие шаги на каждом гипервизоре кластера:

1. Создайте резервную копию текущих сертификатов

```
cp /etc/pki/vdsm/libvirt-vnc/server-key.pem /etc/pki/vdsm/libvirt-vnc/server-key.pem.backup.$(date +%Y%m%d)
cp /etc/pki/vdsm/libvirt-vnc/server-cert.pem /etc/pki/vdsm/libvirt-vnc/server-cert.pem.backup.$(date +%Y%m%d)
```

2. Обновите сертификаты VNC

```
cp -p /etc/pki/vdsm/keys/vdsmkey.pem /etc/pki/vdsm/libvirt-vnc/server-key.pem
cp -p /etc/pki/vdsm/certs/vdsmcert.pem /etc/pki/vdsm/libvirt-vnc/server-cert.pem
```

3. Отключите управления питанием в панели управления, для этого перейдите в Compute → Hosts → \[имя\_хоста] → Edit → Power Management и отключите опцию «Enable Power Management». Это предотвратит непреднамеренное выключение хоста во время перезапуска служб.
4. Для применения изменений перезапустите службу libvirtd

```
systemctl restart libvirtd
```

{% hint style="warning" %}
Перезапуск службы libvirtd временно прервет консольные подключения к работающим виртуальным машинам.
{% endhint %}

5. Вернитесь в настройки управления питанием хоста (Compute → Hosts → \[имя\_хоста] → Edit → Power Management) и включите опцию «Enable Power Management.
6. Проверьте, что сертификат имеет актуальную срок действия&#x20;

```
openssl x509 -in /etc/pki/vdsm/libvirt-vnc/server-cert.pem -noout -dates
```

7. Запустите виртуальные машины, которые не запускались из-за ошибки сертификата.
