# Как проверить срок действия SSL сертификатов

Для обеспечения стабильной работы платформы HOSTVM необходимо контролировать сроки действия сертификатов. Просроченные сертификаты приводят к ошибкам подключения к консоли управления и проблемам с миграцией виртуальных машин. Существует два основных способа проверки: через OpenSSL и с помощью [скрипта Zabbix](https://kb.pvhostvm.ru/hostvm-server/installation-guide/konfiguraciya-podklyucheniya-zabbix).

## Проверка сертификатов через OpenSSL

### Проверка сертификатов HOSTVM Manager

Подключитесь по SSH к HOSTVM Manager и выполните проверку ключевых сертификатов с помощью следующих команд:

```bash
openssl x509 -noout -enddate -in /etc/pki/ovirt-engine/ca.pem
```

```bash
openssl x509 -noout -enddate -in /etc/pki/ovirt-engine/apache-ca.pem
```

```bash
openssl x509 -noout -enddate -in /etc/pki/ovirt-engine/certs/apache.cer
```

```bash
openssl x509 -noout -enddate -in /etc/pki/ovirt-engine/certs/engine.cer
```

**Результат:** команды отобразят даты окончания действия сертификатов.

### Проверка сертификатов HOSTVM Node

Подключитесь по SSH к HOSTVM Node и выполните проверку сертификатов с помощью команд:

```bash
openssl x509 -noout -enddate -in /etc/pki/vdsm/certs/cacert.pem
```

```bash
openssl x509 -noout -enddate -in /etc/pki/vdsm/certs/vdsmcert.pem
```

```bash
openssl x509 -noout -enddate -in /etc/pki/libvirt/clientcert.pem
```

```bash
openssl x509 --noout -enddate -in /etc/pki/vdsm/libvirt-migrate/server-cert.pem
```

**Результат:** команды отобразят даты окончания действия сертификатов.

## Проверка сертификатов через скрипт Zabbix

Если у вас настроена [конфигурация Zabbix-агента](https://kb.pvhostvm.ru/hostvm-server/installation-guide/konfiguraciya-podklyucheniya-zabbix), вы можете проверить сроки действия сертификатов с помощью скрипта, выполнив команды прямо из командной строки менеджера.\
Скрипт обращается к инфраструктуре и собирает данные по всем сертификатам.

### Проверка сертификатов HOSTVM Manager

**В консоли HOSTVM Manager** выполните команду:

```bash
python3 /etc/zabbix/scripts/zbx-hostvm.py hostvm.certificates
```

Эта команда покажет, сколько дней осталось до истечения срока действия сертификатов на HOSTVM Engine.

```
[root@engine ~]# python3 /etc/zabbix/scripts/zbx-hostvm.py hostvm.certificates
[
  {
    "name": "ca.pem",
    "days_remaining": 7290
  },
  {
    "name": "apache-ca.pem",
    "days_remaining": 7290
  },
  {
    "name": "apache.cer",
    "days_remaining": 388
  },
  {
    "name": "engine.cer",
    "days_remaining": 1817
  }
]
```

### Проверка сертификатов HOSTVM Node

**В консоли HOSTVM Manager** выполните команду:

```bash
python3 /etc/zabbix/scripts/zbx-hostvm.py hostvm.host_certificates
```

Эта команда собирает информацию о сертификатах со всех хостов, добавленных в кластер **HOSTVM**.

```
[root@engine ~]# python3 /etc/zabbix/scripts/zbx-hostvm.py hostvm.host_certificates
[
  {
    "name": "node.cer",
    "days_remaining": 1817
  }
]
```

Если срок действия сертификатов подходит к концу, необходимо выполнить их обновление, как описано в [этой статье](https://kb.pvhostvm.ru/hostvm-server/rukovodstvo-po-administrirovaniyu/chasto-zadavaemye-voprosy/kak-peregenerirovat-ssl-sertifikaty-na-hosted-engine-i-khostakh).
