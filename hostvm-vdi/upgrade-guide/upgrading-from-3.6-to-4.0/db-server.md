# Сервер БД

## Обновление виртуальной машины <a href="#vm-update" id="vm-update"></a>

> Процедура обновления существующей установки Сервера БД версии 3.6 до 4.0 без замены виртуальной машины

### Предварительные условия <a href="#prerequisites" id="prerequisites"></a>

1. Остановите службы брокера 3.6, выполнив в его консоли команду:

```shell-session
# systemctl stop vdi.service vdiweb.service
```

2. Сделайте резервную копию БД.

MariaDB:

```shell-session
# mysqldump --single-transaction udsdb > backup.sql
```

PostgreSQL:

```shell-session
# su - postgres
$ pg_dump udsdb > /tmp/udsdb.bak
```

где `udsdb` - имя БД (по умолчанию).

3. Убедитесь в наличии свободного места для установки обновлений.

### Обновление сервера БД <a href="#dbserver-update" id="dbserver-update"></a>

1. Установите последние доступные обновления на текущую версию ОС:

```shell-session
# apt-get update
# apt-get upgrade --yes
```

2. Обновите список источников пакетов в sources.list:

```shell-session
# sed -i -e "s/bullseye/bookworm/g" /etc/apt/sources.list
```

3. Обновите ОС до новой версии:

```shell-session
# apt-get update
# apt-get dist-upgrade --yes
```

4. Удалите неактуальные версии пакетов, оставшиеся после обновления, и перезапустите сервер:

```shell-session
# apt-get autoremove --yes
# apt-get clean
# apt-get autoclean
# reboot
```

#### MariaDB

> Дополнительные действия после обновления при использовании MariaDB

1. Убедитесь, что после обновления версия MariaDB соответствует [системным требованиям](../../hostvm-vdi-installation-guide/requirements/README.md):

```shell-session
# apt-cache policy mariadb-server | grep Installed
  Installed: 1:10.11.14-0+deb12u2
# mariadb --version
mariadb  Ver 15.1 Distrib 10.11.14-MariaDB, for debian-linux-gnu (x86_64) using  EditLine wrapper
```

Обновление сервера БД завершено.

#### PostgreSQL

> Дополнительные действия после обновления при использовании PostgreSQL

> Для конфигурации по умолчанию с использованием кластера 13/main

При обновлении PostgreSQL до версии 15 будет автоматически создан пустой кластер по умолчанию 15/main (13/main - существующий кластер с БД):

```
# pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
13  main    5433 online postgres /var/lib/postgresql/13/main /var/log/postgresql/postgresql-13-main.log
15  main    5432 online postgres /var/lib/postgresql/15/main /var/log/postgresql/postgresql-15-main.log
```

1. Остановите и удалите пустой кластер:

```
# pg_dropcluster 15 main --stop
```

2. Обновите существующий кластер версии 13 до 15:

```
# pg_upgradecluster 13 main
```

После обновления проверьте наличие нового кластера версии 15 в статусе online, старая версия кластера перейдет в статус down:

```
pg_lsclusters
Ver Cluster Port Status Owner    Data directory              Log file
13  main    5433 down   postgres /var/lib/postgresql/13/main /var/log/postgresql/postgresql-13-main.log
15  main    5432 online postgres /var/lib/postgresql/15/main /var/log/postgresql/postgresql-15-main.log
```

Проверьте, что БД состоит в кластере новой версии:

```
# su - postgres
$ psql
postgres=# \c udsdb;
udsdb=# show cluster_name;
 cluster_name 
--------------
 15/main
(1 row)
```

где `udsdb` - имя БД (по умолчанию).

3. Выполните обновление брокера HOSTVM VDI.

4. После успешного обновления брокера удалите старый кластер и пакеты PostgreSQL 13:

```shell-session
# pg_dropcluster 13 main
# apt-get purge postgresql-13 postgresql-client-13
# apt autoremove
```

Обновление сервера БД завершено.
