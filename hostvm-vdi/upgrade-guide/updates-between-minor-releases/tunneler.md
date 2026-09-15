---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
  anchors:
    visible: true
---

# Туннелер HOSTVM VDI

Способы установки минорных обновлений на Туннелер HOSTVM VDI.\
При обновлении на новую мажорную версию обратитесь к соответствующему разделу документации, либо в техническую поддержку.

## Замена виртуальной машины <a href="#tunneler-replace" id="tunneler-replace"></a>

> Данный способ является универсальным для всех версий HOSTVM VDI Tunneler.

Обновление осуществляется путем разворачивания новой версии виртуальной машины туннелера и подключения к брокеру HOSTVM VDI согласно статье ["Установка HOSTVM VDI Tunneler"](../../hostvm-vdi-installation-guide/tunneler/virtual-appliance.md).

При задании сетевых настроек, идентичных старой версии туннелера, дополнительной конфигурации со стороны брокера не требуется.

Если будет использоваться другой IP адрес, необходимо создать соответствующие транспорты для туннельных подключений в панели управления брокера, с использованием нового адреса.

## Обновление виртуальной машины <a href="#tunneler-update" id="tunneler-update"></a>

> Данный способ поддерживается начиная с версии 3.6.

Обновите виртуальную машину:

```shell-session
# apt update
# apt upgrade
```

При необходимости перезапустите ее.

> DEB пакеты доступны для загрузки в личном кабинете HOSTVM.

Загрузите пакет `hostvm-gw_3.6-<номер сборки>.deb` на ВМ и установите обновление:

```shell-session
# apt install /tmp/hostvm-gw_3.6-20241004.deb
```

> Если конфигурация туннелера еще не была выполнена, запустите мастер настройки `hostvm-setup`.

Перезапустите службы:

```shell-session
# systemctl restart vditunnel.service guacd.service tomcat9.service
```

## Установка обновления на РЕД ОС <a href="#gw-redos-update" id="gw-redos-update"></a>

> Данный способ поддерживается начиная с версии 3.6.

Обновление осуществляется путём установки пакета hostvm-gw новой версии.

Скачайте новую версию пакета из личного кабинета, затем выполните установку:

```shell-session
$ sudo yum install ./hostvm-gw-3.6-4.el7.x86_64.rpm
```

## Установка обновления на ALT Linux <a href="#gw-alt-update" id="gw-alt-update"></a>

> Данный способ поддерживается начиная с версии 3.6.

Обновление осуществляется путём установки пакета hostvm-gw новой версии.

Скачайте новую версию пакета из личного кабинета, затем выполните установку:

```shell-session
# apt-get install ./hostvm-gw-3.6-alt4.x86_64.rpm
```
