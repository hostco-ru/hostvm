# Настройка репликации БД между серверами

Данное руководство описывает процесс создания отказоустойчивой конфигурации из двух экземпляров брокера VDI и БД с репликацией master - slave. Его можно использовать как для развертывания новой установки, так и для подключения второго сервера к уже имеющейся.

Данная инструкция подходит как для репликации встроенной БД в appliance брокера, так и внешней БД, подключенной к нему.\
\
В качестве адресов для примера используются:\
_**10.0.0.1**_ - адрес первого брокера\
_**10.0.0.2**_ - адрес второй брокера\
_**10.0.0.3**_ - адрес первой внешней базы данных (если используется)\
_**10.0.0.4**_ - адрес второй внешней базы данных (если используется)

<details>

<summary>Если используются внутренние БД  апплаенса брокера HOSTVM VDI</summary>

## Настройка первого сервера (Master)

**На первом брокере (**_**10.0.0.1):**_

* Перед настройкой реплики остановить:

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

* &#x20;Отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`

В параметре `bind-address` указать:\
IP адрес адрес первого брокера (_**10.0.0.1)**_.

Раскомментировать параметры и задать значения:

```
server-id = 1
log_bin = /var/log/mysql/mysql-bin.log
```

* Перезапустить сервис mariadb

```
systemctl restart mariadb
```

* Зайти в MYSQL с привилегиями root, создать пользователя для репликации, назначить ему необходимые разрешения:

`mysql -p`

`CREATE USER 'replica'@'%' IDENTIFIED BY 'password';`

где `replica` это имя пользователя и `password` - пароль.

`GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%';`

Добавить разрешения для подключения к БД с других хостов:

```
DROP USER 'udsdbadm'@'localhost';
CREATE USER 'udsdbadm'@'%' IDENTIFIED BY 'rom9nsNL';
```

Где 'rom9nsNL' – пароль по умолчанию для пользователя `udsdbadm`. При указании пароля отличного от стандартного необходимо внести соответствующие изменения в файл конфигурации брокера `/var/server/server/setting.py`, а также сменить пароль пользователю `udsdbadm` на новый на втором сервере (Slave).

```
GRANT ALL PRIVILEGES ON *.* TO 'udsdbadm'@'%';
FLUSH PRIVILEGES;
```

* Создать резервную копию БД **(обязательно!**):

```
mysqldump -u root --single-transaction --master-data udsdb > backup.sql
```

* Запустить сервисы брокера:

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

**На втором брокере (10.0.0.2):**

* Остановить сервисы  брокера:

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

* Скопировать файл `/var/server/server/settings.py` с первого (**10.0.0.1**)  брокера с заменой на второй (**10.0.0.2**):
* Запустить сервисы  брокера

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

### Настройка репликации

**На втором брокере (10.0.0.2):**

* отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`
*   В параметре `bind-address` указать IP адрес второго брокера (**10.0.0.2**)&#x20;

    ```
    bind-address = 10.0.0.2
    ```
*   Раскомментировать параметры и задать значения:

    ```
    server-id = 2
    log_bin /var/log/mysql/mysql-bin.log
    ```
*   Перезапустить сервис mariadb

    `systemctl restart mariadb`
*   Скопировать и развернуть дамп базы с первого (**10.0.0.1**) сервера **(обязательно!)**.

    ```bash
    cat backup.sql | /usr/bin/mysql -u root udsdb
    ```
*   Получить параметры `MASTER_LOG_FILE` и `MASTER_LOG_POS` из дампа:

    ```bash
    head -n22 backup.sql | tail -1
    ```

    Пример вывода предыдущей команды:

    `CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`
*   Зайти в MYSQL с привилегиями root

    `mysql -p`
*   Остановить все операции на сервере

    `STOP SLAVE;`
*   Настроить репликацию с первым (_**10.0.0.1**_) сервером

    `CHANGE MASTER TO MASTER_HOST='10.0.0.1', MASTER_USER='replica', MASTER_PASSWORD='password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`

    Где `10.0.0.1` - адрес первого (_**10.0.0.1**_) сервера БД, `replica` -настроенный ранее пользователь для репликации, `password` - его пароль, `myslq-bin.000001` и `236295` - параметры журнала, полученные ранее.
*   Запустить сервер:

    `START SLAVE;`
*   Проверить параметры репликации:

    `SHOW SLAVE STATUS\G`

    В выводе должен быть корректный адрес первого (_**10.0.0.1**_)  сервера `Master_Host: 10.0.0.1` и значение параметров `Slave_IO_Running` и `Slave_SQL_Running` равное `Yes`



</details>

<details>

<summary>Если используются серверы с внешними БД</summary>

## Настройка первого сервера (Master)

_Если при настройке внешних баз данных есть потребность предварительного переноса информации (сервисы, пользователи, транспорты и пр.) с уже настроенной встроенной базы данных брокера HOSTVM VDI - перенесите базу данных на первый (**10.0.0.3**) сервер БД согласно_ [_руководства_](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/vdi-db#perenos-bazy-dannykh) _("Перенос базы данных")._ \
_Если перенос данных не требуется или репликация настраивается с нуля - этот пункт можно пропустить._\
\
**На первом сервере БД (**_**10.0.0.3):**_

* &#x20;Отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`

В параметре `bind-address` указать:\
IP адрес адрес первого сервера  (_**10.0.0.3)**_.

Раскомментировать параметры и задать значения:

```
server-id = 1
log_bin = /var/log/mysql/mysql-bin.log
```

* Перезапустить сервис mariadb

```
systemctl restart mariadb
```

* Зайти в MYSQL с привилегиями root, создать пользователя для репликации, назначить ему необходимые разрешения:

`mysql -p`

`CREATE USER 'replica'@'%' IDENTIFIED BY 'password';`

где `replica` это имя пользователя и `password` - пароль.

`GRANT REPLICATION SLAVE ON *.* TO 'replica'@'%';`

* Добавить разрешения для подключения к БД с других хостов:

```
DROP USER 'udsdbadm'@'localhost';
CREATE USER 'udsdbadm'@'%' IDENTIFIED BY 'rom9nsNL';
```

Где 'rom9nsNL' – пароль по умолчанию для пользователя `udsdbadm`. При указании пароля отличного от стандартного необходимо внести соответствующие изменения в файл конфигурации брокера `/var/server/server/setting.py`, а также сменить пароль пользователю `udsdbadm` на новый на втором сервере (Slave).

```
GRANT ALL PRIVILEGES ON *.* TO 'udsdbadm'@'%';
FLUSH PRIVILEGES;
```

* Создать резервную копию БД **(обязательно!**):

```
mysqldump -u root --single-transaction --master-data udsdb > backup.sql
```

## Настройка брокеров

**На первом брокере (**_**10.0.0.1**_**):**

* Остановить сервисы:

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

* Отредактировать файл `/var/server/server/settings.py`, указать в поле **`HOST`** адрес первого сервера БД (_**10.0.0.3**_):

```
DATABASES = {
...
        'HOST': '10.0.0.3'
...
}
```

* Запустить сервисы:

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

**На втором брокере (**_**10.0.0.2**_**):**

* Остановить сервисы:

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

* Скопировать файл `/var/server/server/settings.py` с первого (_**10.0.0.1**_)  брокера с заменой на второй (**10.0.0.2**):
* Запустить сервисы  брокера:

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

### Настройка репликации

На втором сервере БД (_**10.0.0.4**_):

* отредактировать `/etc/mysql/mariadb.conf.d/50-server.cnf`
*   В параметре `bind-address` указать IP адрес второго сервера БД (**10.0.0.4**)&#x20;

    ```
    bind-address = 10.0.0.4
    ```
*   Раскомментировать параметры и задать значения:

    ```
    server-id = 2
    log_bin /var/log/mysql/mysql-bin.log
    ```
*   Перезапустить сервис mariadb

    `systemctl restart mariadb`
*   Скопировать и развернуть дамп базы с первого (**10.0.0.3**) сервера БД **(обязательно!)**.

    ```bash
    cat backup.sql | /usr/bin/mysql -u root udsdb
    ```
*   Получить параметры `MASTER_LOG_FILE` и `MASTER_LOG_POS` из дампа:

    ```bash
    head -n22 backup.sql | tail -1
    ```

    Пример вывода предыдущей команды:

    `CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`
*   Зайти в MYSQL с привилегиями root

    `mysql -p`
*   Остановить все операции на сервере

    `STOP SLAVE;`
*   Настроить репликацию с первым (_**10.0.0.3**_) сервером БД

    `CHANGE MASTER TO MASTER_HOST='10.0.0.3', MASTER_USER='replica', MASTER_PASSWORD='password', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=236295;`

    Где `10.0.0.3` - адрес первого (_**10.0.0.3**_) сервера БД, `replica` -настроенный ранее пользователь для репликации, `password` - его пароль, `myslq-bin.000001` и `236295` - параметры журнала, полученные ранее.
*   Запустить сервер:

    `START SLAVE;`
*   Проверить параметры репликации:

    `SHOW SLAVE STATUS\G`

    В выводе должен быть корректный адрес первого (_**10.0.0.3**_)  сервера `Master_Host: 10.0.0.3` и значение параметров `Slave_IO_Running` и `Slave_SQL_Running` равное `Yes`



</details>
