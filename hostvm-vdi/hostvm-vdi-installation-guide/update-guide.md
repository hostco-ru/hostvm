---
layout:
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
---

# Обновление компонентов VDI

## HOSTVM VDI брокер

### Замена виртуальной машины <a href="#broker-replace" id="broker-replace"></a>

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

Импортируйте новую версию ВМ брокера согласно статье ["Установка HOSTVM VDI Broker"](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install), до момента авторизации в веб-интерфейсе управления.

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

_Дополнительно_: выполните [настройку SSL сертификатов](hostvm-vdi-ova-install/#ssl-certificates) брокера.

### Обновление виртуальной машины <a href="#broker-update" id="broker-update"></a>

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

При установке обновления версии **3.6-20250314** дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

### Обновление брокера VDI на РЕД ОС <a href="#broker-redos-update" id="broker-redos-update"></a>

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

При установке обновления версии **3.6-4** (номер сборки **20250314**) дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

### Обновление брокера VDI на ALT Linux <a href="#broker-alt-update" id="broker-alt-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновление осуществляется путём установки пакета hostvm-vdi новой версии.

Скачайте новую версию пакета из личного кабинета, установите:

```shell-session
# apt-get install ./hostvm-vdi-3.6-alt4.x86_64.rpm
```

При установке обновления версии **3.6-4** (номер сборки **20250314**) дополнительно выполните следующие команды:

```shell-session
# cd /var/server
# python3 manage.py migrate
```

После завершения установки обновления перезапустите службы:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

## HOSTVM VDI туннелер

### Замена виртуальной машины <a href="#tunneler-replace" id="tunneler-replace"></a>

> Данный способ является универсальным для всех версий HOSTVM VDI Tunneler.

Обновление осуществляется путем разворачивания новой версии виртуальной машины туннелера и подключения к брокеру HOSTVM VDI согласно статье ["Установка HOSTVM VDI Tunneler"](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy).

При задании сетевых настроек, идентичных старой версии туннелера, дополнительной конфигурации со стороны брокера не требуется.

Если будет использоваться другой IP адрес, необходимо создать соответствующие транспорты для туннельных подключений в панели управления брокера, с использованием нового адреса.

### Обновление виртуальной машины <a href="#tunneler-update" id="tunneler-update"></a>

> Данный способ поддерживается только для версии 3.6.

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

### Установка обновления на РЕД ОС <a href="#gw-redos-update" id="gw-redos-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновление осуществляется путём установки пакета hostvm-gw новой версии.

Скачайте новую версию пакета из личного кабинета, затем выполните установку:

```shell-session
$ sudo yum install ./hostvm-gw-3.6-4.el7.x86_64.rpm
```

### Установка обновления на ALT Linux <a href="#gw-alt-update" id="gw-alt-update"></a>

> Данный способ поддерживается только для версии 3.6.

Обновление осуществляется путём установки пакета hostvm-gw новой версии.

Скачайте новую версию пакета из личного кабинета, затем выполните установку:

```shell-session
# apt-get install ./hostvm-gw-3.6-alt4.x86_64.rpm
```

