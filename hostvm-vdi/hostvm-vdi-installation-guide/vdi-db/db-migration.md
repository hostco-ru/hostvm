# Миграция данных брокера VDI из СУБД PostgreSQL в СУБД MariaDB

Если имеется необходимость в использовании HOSTVM VDI совместно с СУБД MariaDB, а брокер VDI развернут на использование СУБД PostgreSQL, следует выполнить миграцию данных брокера и переключить HOSTVM VDI Broker на работу с СУБД MariaDB.

### Предварительные требования:

1\)    Сервер с MariaDB

2\)    В MariaDB создана БД - udsdb

3\)    Если сервер внешний - на нем открыт доступ к БД по сети

4\)    Создан пользователь udsdbadm с паролем

{% hint style="warning" %}
**Все шаги необходимо выполнять от пользователя с правами root.**
{% endhint %}

Если вы хотите произвести миграцию с MariaDB на PostgreSQL,  для подготовки БД PostgreSQL воспользуйтесь инструкцией [<mark style="color:blue;">Настройка СУБД PostgreSQL</mark>](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/vdi-db/postgresql)

### Подготовка к миграции БД

Чтобы создать БД выполните команду:

```
mysql -u root -p -e "CREATE DATABASE udsdb;
```

Для того, чтобы разрешить подключение к БД с внешних адресов необходимо добавить пользователя udsdbadm и выдать ему необходимые права.

Откройте терминальный клиент mysql:

```
mysql -u root -p
```

Для создания пользователя с паролем 'password' выполните команду:

```
CREATE USER 'udsdbadm'@'%' IDENTIFI ED BY 'password';
```

Затем выполните команды:

```
GRANT ALL PRIVILEGES ON *.* to 'udsdbadm'@'%';
FLUSH PRIVILEGES;
```

Выполните выход из терминального клиента mysql:

```
\q
```

Отредактируйте файл /etc/mysql/mariadb.conf.d/50-server.cnf

Необходимо для параметра bind-address указать соответствующий ip-адрес сервера, например, для db1 – 10.20.0.25:

```
bind-address = 10.20.0.25
```

После изменения конфигурационного файла перезагрузите сервис БД командой:

```
systemctl restart mariadb
```

Все готово для миграции.

### Миграция БД

Откройте SSH подключение к Брокеру и отредактируйте секцию DATABASES в файле /var/server/server/settings.py, приведите ее к виду (после секции default добавьте секцию mysql):

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'udsdb',
        'USER': 'udsdbadm',
        'PASSWORD': 'Qwerty_123',
        'HOST': 'localhost',
        'PORT': '',
    },
    'mysql': {
        'ENGINE': 'django.db.backends.mysql',  
        'OPTIONS': {            
            'isolation_level': 'read committed',
        },
        'NAME': 'udsdb', 
        'USER': 'udsdbadm', 
        'PASSWORD': 'password',
        'HOST': 'localhost', 
        'PORT': '3306',  
    }
}
```

Ниже скриншот конфигурации, когда обе БД на одном брокере:

<figure><img src="../../../.gitbook/assets/Скриншот конфигурации, когда обе БД на одном брокере (1).png" alt=""><figcaption></figcaption></figure>

Далее перейдите в директорию /var/server командой:

```
cd /var/server
```

Подготовьте БД командой:

```
python3 manage.py migrate --database=mysql
```

Далее необходимо очистить предзаполненные поля БД для этого необходимо получить SQL код командой:

```
python3 manage.py sqlflush --database=mysql
```

В результате выполнения команды получим следующее:

```
BEGIN;
SET FOREIGN_KEY_CHECKS = 0;
TRUNCATE `uds_userpreference`;
TRUNCATE `uds_log`;
TRUNCATE `uds_authenticator_tags`;
TRUNCATE `uds_group`;
TRUNCATE `uds_osmanager`;
TRUNCATE `uds_images`;
TRUNCATE `uds_accounts_tags`;
TRUNCATE `uds_user`;
TRUNCATE `uds_utility_cache`;
TRUNCATE `uds_transport_tags`;
TRUNCATE `uds_net_trans`;
TRUNCATE `uds__meta_grps`;
TRUNCATE `django_session`;
TRUNCATE `uds_transport`;
TRUNCATE `uds__ds_trans`;
TRUNCATE `uds__meta_pool_member`;
TRUNCATE `uds_scheduler`;
TRUNCATE `uds_calendar_rules`;
TRUNCATE `uds_osmanager_tags`;
TRUNCATE `uds_permissions`;
TRUNCATE `uds_group_users`;
TRUNCATE `uds_tickets`;
TRUNCATE `uds_tunneltoken`;
TRUNCATE `uds_stats_c`;
TRUNCATE `uds__deployed_service_tags`;
TRUNCATE `uds__deployed_service_pub`;
TRUNCATE `uds_proxies`;
TRUNCATE `uds__pool_meta_tags`;
TRUNCATE `uds_mfa`;
TRUNCATE `uds_acc_usage`;
TRUNCATE `uds_dbfile`;
TRUNCATE `uds_accounts`;
TRUNCATE `uds_stats_c_accum`;
TRUNCATE `uds_calendar`;
TRUNCATE `uds__deployed_service`;
TRUNCATE `uds_provider_tags`;
TRUNCATE `uds_service`;
TRUNCATE `uds_delayedtask`;
TRUNCATE `uds_cal_access`;
TRUNCATE `uds__deployed_service_pub_cl`;
TRUNCATE `uds_stats_e`;
TRUNCATE `uds_actortoken`;
TRUNCATE `uds__user_service_property`;
TRUNCATE `uds_storage`;
TRUNCATE `uds_uniqueid`;
TRUNCATE `uds__pools_groups`;
TRUNCATE `uds_configuration`;
TRUNCATE `uds_group_groups`;
TRUNCATE `uds_authenticator`;
TRUNCATE `uds_network`;
TRUNCATE `uds_network_tags`;
TRUNCATE `uds_calendar_tags`;
TRUNCATE `uds_proxies_tags`;
TRUNCATE `uds_service_tags`;
TRUNCATE `uds_provider`;
TRUNCATE `uds__ds_grps`;
TRUNCATE `uds_cal_maccess`;
TRUNCATE `uds__pool_meta`;
TRUNCATE `uds_tag`;
TRUNCATE `uds_mfa_tags`;
TRUNCATE `uds__user_service`;
TRUNCATE `uds_cal_action`;
SET FOREIGN_KEY_CHECKS = 1;
COMMIT;
```

Откройте терминальный клиент mysql:

```
mysql -u root -p
```

Перед выполнением кода выберите БД udsdb командой:

```
use udsdb;
```

Выполните полученный ранее код в БД:

<figure><img src="../../../.gitbook/assets/Выполните полученный ранее код в БД.png" alt=""><figcaption></figcaption></figure>

Выполните выход из терминального клиента mysql:

```
\q
```

Создайте dump БД PostgreSQL в универсальном JSON формате командой:

```
python3 manage.py dumpdata --all --natural-foreign > dump.json
```

Теперь загрузите dump в БД MariaDB командой:

```
python3 manage.py loaddata dump.json --database=mysql
```

Необходимо переключить HOSTVM VDI Broker с БД PostgreSQL на MariaDB.

В файле /var/server/server/settings.py необходимо удалить конфигурацию default в разделе DATABASES и заменить mysql на default, чтобы получилось, как на скриншоте:

<figure><img src="../../../.gitbook/assets/Необходимо переключить Django с БД postgreSQL на mariadb.png" alt=""><figcaption></figcaption></figure>

Теперь необходимо перезапустить службы брокера командой:

```
systemctl restart vdi vdiweb
```

Миграция завершена!
