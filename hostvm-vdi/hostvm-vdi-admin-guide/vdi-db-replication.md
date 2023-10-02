# Настройка репликации БД между серверами

Данное руководство описывает процесс создания отказоустойчивой конфигурации из двух экземпляров брокера VDI и БД с репликацией master - slave. Его можно использовать как для развертывания новой установки, так и для подключения второго сервера к уже имеющейся.

Данная инструкция подходит как для репликации встроенной БД в appliance брокера, так и внешней БД, подключенной к нему.

## Настройка первого сервера (Master)

Перед настройкой реплики остановить сервисы брокера:

{% code title="Брокер версии 3.0" %}
```bash
systemctl stop apache2 uds
```
{% endcode %}

{% code title="Брокер версии >= 3.5" %}
```
systemctl stop vdi.service vdiweb.service
```
{% endcode %}

На сервере master БД отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`

В параметре `bind-address` указать IP адрес первого брокера или внешней базы данных, если используется:

```
bind-address = 10.0.0.1
```

Раскомментировать параметры и задать значения:

```
server-id = 1
log_bin = /var/log/mysql/mysql-bin.log
```

Перезапустить сервис на сервере  с БД:

```bash
systemctl restart mariadb
```

Зайти в MYSQL с привилегиями root, создать пользователя для репликации, назначить ему необходимые разрешения:

`mysql -p`

`CREATE USER 'replica'@'%' IDENTIFIED BY 'password';`

где `replica` это имя пользователя и `password` - пароль.

`GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%';`

Добавить разрешения для подключения к БД с других хостов:

```
UPDATE mysql.user SET Host='%' WHERE Host='localhost' AND User='udsdbadm';
FLUSH PRIVILEGES;
```

Создать резервную копию БД **(обязательно!**):

```bash
mysqldump -u root --single-transaction --master-data udsdb > backup.sql
```

Запустить сервисы брокера:

{% code title="Брокер версии 3.0" %}
```bash
systemctl start apache2 uds
```
{% endcode %}

{% code title="Брокер версии >= 3.5" %}
```
systemctl start vdi.service vdiweb.service
```
{% endcode %}

## Настройка второго сервера (Slave)

### Подключение брокера к БД master сервера

Остановить сервисы брокера:

{% code title="Брокер версии 3.0" %}
```bash
systemctl stop apache2 uds
```
{% endcode %}

{% code title="Брокер версии >= 3.5" %}
```
systemctl stop vdi.service vdiweb.service
```
{% endcode %}

Скопировать файл `/var/server/server/settings.py` с первого  брокера:

Запустить сервисы брокера:

{% code title="Брокер версии 3.0" %}
```bash
systemctl start apache2 uds
```
{% endcode %}

{% code title="Брокер версии >= 3.5" %}
```
systemctl stop vdi.service vdiweb.service
```
{% endcode %}

### Настройка репликации

На сервере slave БД отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`

В параметре `bind-address` указать IP адрес второго сервера БД

```
bind-address = 10.0.0.2
```

Раскомментировать параметры и задать значения:

```
server-id = 2
log_bin /var/log/mysql/mysql-bin.log
```

Перезапустить сервис на сервере slave БД

`systemctl restart mariadb`

Скопировать на сервер и развернуть дамп базы с первого сервера **(обязательно!)**.

```bash
cat backup.sql | /usr/bin/mysql -u root udsdb
```

Получить параметры `MASTER_LOG_FILE` и `MASTER_LOG_POS` из дампа:

```bash
head -n22 backup.sql | tail -1
```

Пример вывода предыдущей команды:

`CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`

Зайти в MYSQL с привилегиями root

`mysql -p`

Остановить все операции на сервере

`STOP SLAVE;`

Настроить репликацию с первым сервером

`CHANGE MASTER TO MASTER_HOST='10.0.0.1', MASTER_USER='replica', MASTER_PASSWORD='password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`

Где `10.0.0.1` - адрес первого сервера БД, `replica` -настроенный ранее пользователь для репликации, `password` - его пароль, `myslq-bin.000001` и `236295` - параметры журнала, полученные ранее.

Запустить сервер:

`START SLAVE;`

Проверить параметры репликации:

`SHOW SLAVE STATUS\G`

В выводе должен быть корректный адрес первого сервера `Master_Host: 10.0.0.1` и значение параметров `Slave_IO_Running` и `Slave_SQL_Running` равное `Yes`
