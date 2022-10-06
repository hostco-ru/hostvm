# Установка и настройка выделенного сервера БД

## Подготовка сервера БД

В качестве ОС для установки на сервер используйте дистрибутив Debian Buster (10.12 или выше). Установка с образа netinstall (требуемые компоненты: ssh server, standard system utilities).

Минимальные требования к ВМ:

* Диск: 10 Гб
* Память: 2048 Мб
* vCPU: 1

После завершения установки поставить пакет:

```bash
apt install mariadb-server
```

И запустить сценарий конфигурации для защиты базы данных:

```bash
mysql_secure_installation
```

Мастер установки попросит ввести текущий пароль для пользователя root, так как для выполнения процесса потребуются права администратора.

При возникновении уведомления об изменении пароля пользователя root следует выбрать вариант «Нет» («No»).

При возникновении уведомления об удалении существующих анонимных пользователей следует выбрать вариант «Да» («Yes»).

При возникновении уведомления об отключении возможности входа для пользователя root удаленно следует выбрать вариант «Да» («Yes»).

При возникновении уведомления об удалении тестовой базы данных следует выбрать вариант «Да» («Yes»).

При возникновении уведомления о перезагрузке таблицы привилегий следует выбрать вариант «Да» («Yes»).

После завершения процесса создать пользователя, которым будет подключаться брокер:

```bash
mysql -u root
CREATE DATABASE vdidb CHARACTER SET utf8;
CREATE USER 'vdidbadm'@'%' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO 'vdidbadm'@'%';
FLUSH PRIVILEGES;
\q
```

`vdidb` - имя БД

`vdidbadm` - имя пользователя, укажите произвольное

`password` - пароль, укажите другой, соответствующий требованиям безопасности

Внесите изменения в файл `/etc/mysql/mariadb.conf.d/50-server.cnf`:

```
bind-address            = 0.0.0.0
```

И перезапустите сервер БД:

```bash
systemctl restart mariadb
```

## Перенос БД

На машине брокера VDI выполните команды:

```bash
systemctl stop apache2 uds
mysqldump -u root --single-transaction udsdb > backup.sql
```

Перенесите резервную копию базы `backup.sql` на сервер БД, разверните ее:

```
cat backup.sql | /usr/bin/mysql -u root vdidb
```

`vdidb` - имя БД

## Настройка брокера VDI

Внесите изменения в файл `/var/server/server/settings.py`:

```
DATABASES = {
...
    'NAME': 'vdidb',
    'USER': 'vdidbadm',
    'PASSWORD': 'password',
    'HOST': '10.1.1.1',
    'PORT': '3306',
```

`vdidb` - имя БД, аналогично созданной новой базе на сервере БД

`vdidbadm` - имя пользователя, аналогично созданному на сервере БД

`password` - пароль, аналогично заданному на сервере БД

`10.1.1.1` - IP адрес сервера БД

Запустите сервисы брокера:

```bash
systemctl start apache2 uds
```