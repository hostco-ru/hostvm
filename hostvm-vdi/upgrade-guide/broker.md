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
---

# Брокер HOSTVM VDI

## Замена виртуальной машины <a href="#broker-replace" id="broker-replace"></a>

> Данный способ является универсальным для всех версий HOSTVM VDI Broker.

> В примере описана процедура обновления версии сборки в рамках мажорной версии 3.6, для конфигурации "один брокер со встроенной БД MariaDB". В случае обновления других конфигураций, а также при обновлении на новую мажорную версию брокера, обратитесь в техническую поддержку.

Обновление осуществляется путем переноса базы и настроек на новую ВМ брокера. В момент переноса портал и сервисы пользователей будут недоступны.

Создайте резервную копию БД:

```shell-session
# mysqldump -u root --single-transaction udsdb > backup.sql
```

Скопируйте файл резервной копии на внешний ресурс.

Сохраните файл конфигурации брокера `/var/server/server/settings.py`

Сохранить сетевые настройки из файла `/etc/network/interfaces` и имя машины, если их планируется переносить на новую ВМ.

Выключите ВМ.

Импортируйте новую версию ВМ брокера согласно статье ["Установка HOSTVM VDI Broker"](../hostvm-vdi-installation-guide/broker/virtual-appliance.md), до момента авторизации в веб-интерфейсе управления.

Настройки сети и имя машины задайте от старой версии ВМ брокера, если применимо.

Скопируйте с внешнего ресурса и разверните резервную копию БД:

```shell-session
# systemctl stop vdi.service
# cat backup.sql | /usr/bin/mysql -u root udsdb
```

Перенесите сохраненный файл конфигурации брокера `/var/server/server/settings.py` на ВМ.

Перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

После успешного запуска служб портал, конфигурация и сервисы пользователей будут доступны через веб-интерфейс брокера.

_Дополнительно_: выполните [настройку SSL сертификатов](../hostvm-vdi-installation-guide/broker/virtual-appliance.md#ssl-certificates) брокера.

## Обновление виртуальной машины <a href="#broker-update" id="broker-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновите виртуальную машину:

```shell-session
# apt update
# apt upgrade
```

При необходимости перезапустите ее.

> DEB пакеты доступны для загрузки в личном кабинете HOSTVM.

Загрузите пакет `hostvm-vdi_3.6-<номер сборки>.deb` на ВМ и установите обновление:

```shell-session
# apt install /tmp/hostvm-vdi_3.6-20241015.deb
```

{% hint style="warning" %}
При установке обновления версии **3.6-20250314** дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```
{% endhint %}

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

## Установка обновления на РЕД ОС <a href="#broker-redos-update" id="broker-redos-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновление осуществляется путём установки пакета hostvm-vdi новой версии.

Скачайте и разархивируйте пакет из личного кабинета:

```shell-session
$ tar -xvf ./hostvm-vdi<номер версии>-<номер сборки>.rpm.tar.bz2
```

Установите скачанный пакет:

```shell-session
$ sudo yum install ./hostvm-vdi-3.6-4.el7.x86_64.rpm --allowerasing
```

{% hint style="warning" %}
При установке обновления версии **3.6-4** (номер сборки **20250314**) дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```
{% endhint %}

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

## Установка обновления на ALT Linux <a href="#broker-alt-update" id="broker-alt-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновление осуществляется путём установки пакета hostvm-vdi новой версии.

Скачайте новую версию пакета из личного кабинета, установите:

```shell-session
# apt-get install ./hostvm-vdi-3.6-alt4.x86_64.rpm
```

{% hint style="warning" %}
При установке обновления версии **3.6-4** (номер сборки **20250314**) дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```
{% endhint %}

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

