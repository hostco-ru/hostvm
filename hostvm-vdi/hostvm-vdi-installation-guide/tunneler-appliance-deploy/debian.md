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
4. [Портал пользователя](../tunneler-appliance-deploy.md#user-portal)

