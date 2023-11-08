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

# Настройка мультидоменного доступа

HOSTVM VDI позволяет использовать несколько доменных имен для доступа к одному окружению (с помощью TLS SNI).

Вам понадобятся действительные сертификаты для каждого доменного имени, используемого для доступа к брокеру, в формате `PEM`.

В примере ниже описана настройка доступа к брокеру с использованием двух доменных имен. Для настройки потребуются файлы сертификата (открытой части) и файлы приватного ключа, для каждого настраиваемого домена.

## Версия брокера 3.5 и выше

### Настройка первого домена

Скопируйте открытую и закрытую части сертификата на брокер в директорию `/etc/certs`.

Отредактируйте файл с настройками SSL `/etc/nginx/snippets/vdi-ssl-params.conf`, в директивах `ssl_certificate` и `ssl_certificate_key` укажите новые имена файлов сертификата:

```
ssl_certificate /etc/certs/server.pem; #открытая часть
ssl_certificate_key /etc/certs/key.pem; #приватный ключ
```

Отредактируйте файл конфигурации веб-сервера `/etc/nginx/sites-available/hostvm`, в директиве `server_name` укажите доменное имя брокера, например:

```
server {
...
    server_name hostvm-vdi35.localdomain;
...
    }
```

Чтобы проверить новую конфигурацию, выполните команду `nginx -t`, вывод команды в случае прохождения проверки:

```shell-session
# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Чтобы применить новые настройки, выполните команду:

```shell-session
# systemctl reload nginx
```

### Настройка второго домена

Скопируйте открытую и закрытую части дополнительного сертификата на брокер в директорию `/etc/certs`.

Создайте копию файла `/etc/nginx/snippets/vdi-ssl-params.conf`, например:

```shell-session
# cp /etc/nginx/snippets/vdi-ssl-params.conf /etc/nginx/snippets/vdi-ssl-params-ext.conf
```

Отредактируйте получившийся файл `/etc/nginx/snippets/vdi-ssl-params-ext.conf`, в директивах `ssl_certificate` и `ssl_certificate_key` укажите имена файлов дополнительного сертификата:

```
ssl_certificate /etc/certs/server-ext.pem; #открытая часть
ssl_certificate_key /etc/certs/key-ext.pem; #приватный ключ
```

Скопируйте блок настроек `server` из текущего файла конфигурации `/etc/nginx/sites-available/hostvm`:

```
server {
... # скопируйте все содержимое этого блока
    }
```

Сохраните конфигурацию в новый файл, например `/etc/nginx/sites-available/hostvm-ext`.

Отредактируйте его, удалите параметр `default_server` для директив `listen`, в директиве `server_name` укажите дополнительное доменное имя брокера, для которого будет использоваться другой сертификат, в директиве `include` измените путь к файлу с настройками SSL для нового сертификата. Пример настройки:

```
server {
    listen 80;
    listen [::]:80;
...
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
...
    include snippets/vdi-ssl-params-ext.conf;
...
    server_name hostvm-vdi35-ext.localdomain;
...
    }
```

Остальные параметры оставьте без изменений.

После завершения настройки активируйте конфигурацию:

```shell-session
# ln -s /etc/nginx/sites-available/hostvm-ext /etc/nginx/sites-enabled/
```

Чтобы проверить новую конфигурацию, выполните команду `nginx -t`, вывод команды в случае прохождения проверки:

```shell-session
# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Чтобы применить новые настройки, выполните команду:

```shell-session
# systemctl reload nginx
```

Убедитесь, что портал брокера доступен по обоим URL (`https://hostvm-vdi35.localdomain` и `https://hostvm-vdi35-ext.localdomain`), и в каждом случае отдается соответствующий имени сертификат.
