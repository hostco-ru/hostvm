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

# Настройка СУБД PostgreSQL

{% hint style="warning" %}
**Поддерживаемые версии PostgreSQL: 13, 15**
{% endhint %}

### Мастер установки сервера БД <a href="#db-wizard" id="db-wizard"></a>

В консоли сервера БД выполните команду `dbserver-pg-setup.sh` для запуски мастера установки.

Ответьте на вопросы мастера, задайте пароль пользователя БД для подключения:

```shell-session
# dbserver-pg-setup.sh
Добро пожаловать в мастер настройки DB Server. Пожалуйста, заполните все поля корректными значениями. Для принятия значений по умолчанию нажимайте ENTER.
Настроить сервер на использование базы данных PostgreSQL? [Y/n]: y
Внимание! Предыдущая конфигурация будет удалена, подтвердите операцию: [Y/n] y
Введите пароль пользователя БД: 
Введите (еще раз) пароль пользователя БД: 
Для применения новых параметров и перезапуска служб нажмите ENTER []: 
```

После завершения работы мастера на сервере будет развернут инстанс базы данных брокера VDI с именем `udsdb`, и создан пользователь PostgreSQL `udsdbadm` с правами доступа к этой базе, с заданным в процессе настройки паролем.

Далее, в файле `/etc/postgresql/13/main/postgresql.conf` раскомментируйте строку:

```
listen_addresses = '*'
```

В файл `/etc/postgresql/13/main/pg_hba.conf` разрешите удаленные подключения к БД с брокера VDI, добавив запись с его адресом или подсетью (в примере `10.1.1.0/24`):

```
# IPv4 remote connections:
host    all             all             10.1.1.0/24           md5
```

Для применения новой конфигурации перезапустите службу PostgreSQL:

```shell-session
# systemctl restart postgresql.service
```

### Перенос базы данных <a href="#db-migration" id="db-migration"></a>

В случае новой установки данный раздел настройки можно пропустить.

Для переноса существующей базы данных PostgreSQL, уже развернутой в составе компонента HOSTVM VDI Брокер, на внешний сервер БД, выполните следующие команды.

В консоли брокера VDI остановите сервисы и создайте резервную копию БД, указав:

* имя базы на брокере (по умолчанию `udsdb`)
* имя файла резервной копии (в примере - `udsdb.bak`)

Команды для брокера версии `3.6`:

```shell-session
# systemctl stop vdi.service vdiweb.service
# su - postgres
$ pg_dump udsdb > /tmp/udsdb.bak
$ exit
```

Перенесите файл резервной копии на сервер БД, указав:

* имя файла резервной копии (в примере - `udsdb.bak`)
* IP или hostname сервера БД (в примере `dbserver`)

```shell-session
# scp /tmp/udsdb.bak root@dbserver:/tmp/
```

В консоли сервера БД выполните следующие команды.

Откройте терминальный клиент psql:

```shell-session
# su - postgres
$ psql
```

Пересоздайте БД брокера (в примере - `udsdb`):

```shell-session
postgres=# DROP DATABASE udsdb; CREATE DATABASE udsdb;
postgres=# \q
```

Импортируйте ранее созданную резервную копию БД:

```shell-session
$ psql -d udsdb -f /tmp/udsdb.bak
$ exit
```

В файле `/etc/postgresql/13/main/postgresql.conf` раскомментируйте строку:

```
listen_addresses = '*'
```

В файл `/etc/postgresql/13/main/pg_hba.conf` разрешите удаленные подключения к БД с брокера VDI, добавив запись с его адресом или подсетью (в примере `10.1.1.0/24`):

```
# IPv4 remote connections:
host    all             all             10.1.1.0/24           md5
```

Для применения новой конфигурации перезапустите службу PostgreSQL:

```shell-session
# systemctl restart postgresql.service
```

### Переключение брокера VDI на внешний сервер БД <a href="#broker-config" id="broker-config"></a>

Отредактируйте файл настроек брокера `/var/server/server/settings.py`, в блок `DATABASES` внесите информацию:

<pre><code><strong>DATABASES = {
</strong>    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'OPTIONS': {
            'isolation_level': 'read committed',
        },
        'NAME': 'udsdb',                     # имя БД, по умолчанию udsdb
        'USER': 'udsdbadm',                  # имя пользователя БД, по умолчанию udsdbadm
        'PASSWORD': 'password',              # пароль пользователя БД, как был задан в мастере установки сервера БД
        'HOST': '10.1.1.1',                  # IP адрес сервера БД, например 10.1.1.1
        'PORT': '',                          # порт сервера БД, оставьте пустым при использовании порта по умолчанию
    }
}
</code></pre>

Перезапустите службы брокера VDI:

```shell-session
# systemctl restart vdi.service vdiweb.service
```

Для инициализации базы данных выполните:

<pre class="language-shell-session"><code class="lang-shell-session"># systemctl stop vdi
<strong># cd /var/server
</strong># python3 manage.py migrate
# python3 manage.py createcachetable
# systemctl start vdi
</code></pre>

Убедитесь, что портал брокера VDI успешно открывается в браузере.

#### Дополнительные действия при переносе существующей БД <a href="#cleanup" id="cleanup"></a>

На портале брокера VDI перейдите в панель администрирования, проверьте наличие данных перенесенной конфигурации (настройки сервис-провайдеров, аутентификаторов, транспортов, сервис-пулов и т.д.).

Удалите файл резервной копии с сервера БД, выполнив команду:

```shell-session
# rm /tmp/udsdb.bak
```

Удалите файл резервной копии и отключите более не используемый локальный сервис PostgreSQL на брокере VDI, выполнив команды:

```shell-session
# rm /tmp/udsdb.bak
# systemctl stop postgresql.service
# systemctl disable postgresql.service
```

### Шифрование трафика между брокером VDI и СУБД

В файл `/etc/postgresql/13/main/pg_hba.conf` внесите соответствующую запись с адресом или подсетью брокера(в примере `10.1.1.0/24`):

```
# IPv4 remote connections:
hostssl    all             all             10.1.1.0/24           md5
```

Для применения новой конфигурации перезапустите службу PostgreSQL:

```shell-session
# systemctl restart postgresql.service
```

Отредактируйте файл настроек брокера `/var/server/server/settings.py`, в блок `DATABASES` в секции `OPTIONS` внесите  запись `'sslmode': 'require',`

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'OPTIONS': {
            'isolation_level': 'read committed',
            'sslmode': 'require',
        },
        'NAME': 'udsdb',                     # имя БД, по умолчанию udsdb
        'USER': 'udsdbadm',                  # имя пользователя БД, по умолчанию udsdbadm
        'PASSWORD': 'password',              # пароль пользователя БД, как был задан в мастере установки сервера БД
        'HOST': '10.1.1.1',                  # IP адрес сервера БД, например 10.1.1.1
        'PORT': '',                          # порт сервера БД, оставьте пустым при использовании порта по умолчанию
    }
}
```

Перезапустите службы брокера VDI:

```shell-session
# systemctl restart vdi.service vdiweb.service
```
