# Установка на Debian

## Системные требования <a href="#requirements" id="requirements"></a>

* Операционная система Debian 11 (Bullseye)

## Подготовка системы <a href="#preparation" id="preparation"></a>

Обновите систему:

```shell-session
# apt-get update
# apt-get upgrade
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета пакет туннелера `hostvm-gw_3.6-<номер сборки>.deb` и установите его:

```shell-session
# apt install /tmp/hostvm-gw_3.6-20250314.deb
```

При первичной установке подтвердите перезапись конфигурационного файла:

```shell-session
Configuration file '/etc/tomcat9/server.xml'
 ==> File on system created by you or by a script.
 ==> File also in package provided by package maintainer.
   What would you like to do about it ?  Your options are:
    Y or I  : install the package maintainer's version
    N or O  : keep your currently-installed version
      D     : show the differences between the versions
      Z     : start a shell to examine the situation
 The default action is to keep your current version.
*** server.xml (Y/I/N/O/D/Z) [default=N] ? y
```

Для дальнейшей настройки обратитесь к разделам:

1. [Настройка сертификатов](../tunneler-appliance-deploy.md#ssl-certificates-35)
2. [Мастер установки HOSTVM VDI Tunneler](../tunneler-appliance-deploy.md#setup-wizard)
3. [Настройка транспорта в панели управления HOSTVM VDI](../tunneler-appliance-deploy.md#nastroika-transporta-v-paneli-upravleniya-hostvm-vdi)

## Портал пользователя <a href="#user-portal" id="user-portal"></a>

> Данный функционал доступен начиная с версии 3.6

По умолчанию при установке HOSTVM VDI Tunneler портал пользователя не доступен.

Скачайте из личного кабинета пакет `hostvm-vdi-broker-gui` для Debian, установите его:

```shell-session
# apt install ./hostvm-vdi-broker-gui_3.6-20250318_amd64.deb
```

Для активации портала запустите мастер настройки, выполнив команду `hostvm-vdi-gui-setup`

Задайте запрашиваемые мастером параметры конфигурации:

* IP адрес или FQDN брокера HOSTVM VDI

**Пример конфигурации:**

```shell-session
# hostvm-vdi-gui-setup 
** Добро пожаловать в мастер настройки HOSTVM VDI Broker Web UI. Пожалуйста, заполните все поля корректными значениями. Для принятия значений по умолчанию нажимайте ENTER.

** Введите IP адрес или FQDN брокера HOSTVM VDI []: hostvm-vdi36
** Перезаписать конфигурацию? [Y/n]: y
** Конфигурация обновлена, задан адрес брокера: hostvm-vdi36
** Настройка завершена.
```

После завершения настройки портал будет доступен по адресу `https://<IP или FQDN туннелера>:1443/`

Для настройки SSL сертификатов портала воспользуйтесь соответствующим разделом руководства по установке брокера HOSTVM VDI:

[Настройка SSL сертификатов](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-ova-install#ssl-certificates)

