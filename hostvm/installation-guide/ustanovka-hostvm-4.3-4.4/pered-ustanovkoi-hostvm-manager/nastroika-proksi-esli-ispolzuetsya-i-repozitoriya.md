# Настройка прокси (если используется) и репозитория

{% hint style="warning" %}
Примечание: данный способ настройки прокси может быть применён только для HOSTVM версии 4.3, для HOSTVM 4.4 при наличии прокси необходимо использовать образ с поддержкой установки в offline-режиме.
{% endhint %}

Если в данной сети доступ во вне доступен только через прокси, то следует выполнить следующие настройки:

1. В файле /etc/yum.conf, для загрузки пакетов через прокси добавляем строки

```
proxy=http://proxyhost:8080
proxy_username=proxyname
proxy_password=proxypass
```

2. Экспортировать глобальные переменные прокси для работы CURL

```
## http прокси с именем и паролем 
export http_proxy=http://user:password@your-proxy-ip-address:port/
## HTTPS версия ##
export https_proxy=https://your-ip-address:port/
export https_proxy=https://user:password@your-proxy-ip-address:port/
```

На примере, сначала устанавливаем переменные для прокси и затем используем CURL

```
export http_proxy=http://foo:bar@1.1.1.1:3128/
export https_proxy=$http_proxy
## Use curl command ##
curl -I www.system-admins.ru
```

Если на этапе установки возникала проблема с postinstall скриптом, то необходимо выполнить скрипт `initial.sh`

```
[root@virt2 ~]# sh initial.sh
```

По итогу выполнения скрипта, в директории /root/ будет создан лог-файл `initial_log_текущая-дата.log`
