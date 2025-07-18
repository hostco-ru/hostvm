# Установка на РЕД ОС

## Системные требования <a href="#requirements" id="requirements"></a>

* Операционная система РЕД ОС версии 7.3.4 с отключенной системой контроля доступа SELinux.
* Требуемое базовое окружение: **Сервер с графическим интерфейсом** или **Рабочая станция с графическим интерфейсом.**

## Подготовка системы <a href="#preparation" id="preparation"></a>

Отключить SELinux:

```
sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
sudo setenforce 0
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета и установите пакет туннелера:

```shell-session
# yum install ./hostvm-gw-3.6-3.el7.x86_64.rpm
```

{% hint style="info" %}
При установке версии 3.6-2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:

```shell-session
# /usr/share/hostvm/postinst.sh
```

Начиная с версии 3.6-3 данный шаг не требуется.
{% endhint %}

В ходе установки возможно появление уведомления о приглашении к вводу версии java, которая будет использована. Необходимо указать java-11.

### Сертификаты <a href="#certificates" id="certificates"></a>

Для подключения к брокеру HOSTVM VDI по протоколу HTTPS требуется наличие установленного действительного сертификата на ВМ брокера. При необходимости, перед запуском мастера установки добавьте CA сертификат в доверенные.

Для этого скопируйте его на машину HOSTVM VDI туннелера и поместите в директорию:

```
/etc/pki/ca-trust/source/anchors/
```

Затем выполните команду:

```shell-session
# update-ca-trust
```

### Мастер установки <a href="#setup-wizard" id="setup-wizard"></a>

**Версия 3.6-5 и выше**

Выполните команду `hostvm-vdi setup` для запуски мастера установки.

В процессе установки будет сгенерирован самоподписанный сертификат для туннелера. Вы можете заменить его на собственный: [Сертификат для HTML5 подключений](../tunneler-appliance-deploy.md#html5-certificate). При наличии сертификата в системе (например при настройке существующей инсталляции) вы сможете сохранить старый сертификат либо сгенерировать новый.

**Версия 3.6-4 и ниже**

Выполните команду `hostvm-setup` для запуска мастера установки.

Задайте запрашиваемые мастером параметры конфигурации:

* FQDN брокера HOSTVM VDI;
* Тип соединения с брокером: https;
* Учетная запись с правами администратора на брокере (аутентификатор, логин, пароль). Для использования встроенной учетной записи администратора оставьте значение аутентификатора пустым, нажав Enter.

## Портал пользователя <a href="#user-portal" id="user-portal"></a>

> Данный функционал доступен начиная с версии 3.6

По умолчанию при установке HOSTVM VDI Tunneler портал пользователя не доступен.

Скачайте из личного кабинета пакет `hostvm-vdi-broker-gui` для РЕД ОС, установите его:

```shell-session
# yum install ./hostvm-vdi-broker-gui-3.6-2.el7.x86_64.rpm
```

Для активации портала запустите мастер настройки, выполнив команду `hostvm-vdi-gui-setup`

Задайте запрашиваемые мастером параметры конфигурации:

* IP адрес или FQDN брокера HOSTVM VDI

**Пример конфигурации:**

{% code overflow="wrap" %}
```shell-session
# hostvm-vdi-gui-setup 
** Добро пожаловать в мастер настройки HOSTVM VDI Broker Web UI. Пожалуйста, заполните все поля корректными значениями. Для принятия значений по умолчанию нажимайте ENTER.

** Введите IP адрес или FQDN брокера HOSTVM VDI []: hostvm-vdi36
** Перезаписать конфигурацию? [Y/n]: y
** Конфигурация обновлена, задан адрес брокера: hostvm-vdi36
** Настройка завершена.
```
{% endcode %}

После завершения настройки портал будет доступен по адресу `https://<IP или FQDN туннелера>:1443/`

Для настройки SSL сертификатов портала воспользуйтесь соответствующим разделом руководства по установке брокера HOSTVM VDI:

[Настройка SSL сертификатов](../hostvm-vdi-ova-install/#ssl-certificates)

