# Настройка кластера PostgreSQL

Данное руководство описывает процесс создания отказоустойчивой конфигурации из балансировщика (HAProxy) и нечетного количества экземпляров (три и более) БД PostgreSQL, где один из узлов кластера выступает в качестве Leader, а остальные узлы в качестве Replica. Переключение ролей при утрате доступа к Leader-серверу может производиться как автоматически, так и вручную.

Руководство можно использовать как для развертывания новой установки, так и для подключения серверов к уже имеющейся.

### Действия перед настройкой <a href="#preparation" id="preparation"></a>

Подключитесь к консоли брокера VDI и остановите сервисы:

```
systemctl stop vdi.service vdiweb.service
```

Подключитесь к БД и создайте резервную копию, указав:

* имя базы (по умолчанию udsdb)
* имя файла резервной копии (в примере - udsdb.bak)

```
# su - postgres
$ pg_dump udsdb > /tmp/udsdb.bak
$ exit
```

### Настройка PostgreSQL <a href="#config" id="config"></a>

Создайте роль replicator для работы с репликами и задайте пароль пользователю postgres:

```
su - postgres
psql -c "CREATE USER replicator WITH REPLICATION ENCRYPTED PASSWORD 'engine'"
psql -c "ALTER ROLE postgres PASSWORD 'engine'"
exit
```

Данные пользователи и их пароли указываются в конфигурационном файле Patroni в блоках postgresql и pg\_hba.conf

### Подготовка кластера etcd <a href="#etcd" id="etcd"></a>

{% hint style="warning" %}
Для корректной работы автоматического переключения с Replica на Master количество узлов должно быть нечетным.
{% endhint %}

В описанной конфигурации используется 3 сервера БД:

* postgres-server1 с ролью master (Leader)
* postgres-server2 с ролью slave (Replica)
* postgres-server3 с ролью slave (Replica)

1. Установите etcd на все узлы:

```
apt-get install etcd -y
```

2. Остановите etcd на всех узлах:

```
systemctl stop etcd
```

3. Удалите каталог данных:

```
rm -rf /var/lib/etcd/*
```

4. Переместите конфигурационный файл по умолчанию:

```
mv /etc/default/etcd{,.original}
```

5. Создайте и откройте для редактирования новый конфигурационный файл:

```
nano /etc/default/etcd
```

6. Добавьте пример конфигураций в файл для узла postgres-server1.your\_domain:

```
ETCD_NAME="postgres-server1"
ETCD_DATA_DIR="/var/lib/etcd/default"
ETCD_HEARTBEAT_INTERVAL="1000"
ETCD_ELECTION_TIMEOUT="5000"
ETCD_LISTEN_PEER_URLS="http://10.1.99.2:2380"
ETCD_LISTEN_CLIENT_URLS="http://10.1.99.2:2379,http://localhost:2379"
ETCD_INITIAL_ADVERTISE_PEER_URLS="http://10.1.99.2:2380"
ETCD_INITIAL_CLUSTER="postgres-server1=http://10.1.99.2:2380,postgres-server2=http://10.1.99.3:2380,postgres-server3=http://10.1.99.4:2380"
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-postgres-cluster"
ETCD_ADVERTISE_CLIENT_URLS="http://10.1.99.2:2379"
ETCD_ENABLE_V2="true"
ETCD_INITIAL_ELECTION_TICK_ADVANCE="false"
```

7. Добавьте пример конфигураций в файл для узла postgres-server2.your\_domain:

```
ETCD_NAME="postgres-server2"
ETCD_DATA_DIR="/var/lib/etcd/default"
ETCD_HEARTBEAT_INTERVAL="1000"
ETCD_ELECTION_TIMEOUT="5000"
ETCD_LISTEN_PEER_URLS="http://10.1.99.3:2380"
ETCD_LISTEN_CLIENT_URLS="http://10.1.99.3:2379,http://127.0.0.1:2379"
ETCD_INITIAL_ADVERTISE_PEER_URLS="http://10.1.99.3:2380"
ETCD_INITIAL_CLUSTER="postgres-server1=http://10.1.99.2:2380,postgres-server2=http://10.1.99.3:2380,postgres-server3=http://10.1.99.4:2380"
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-postgres-cluster"
ETCD_ADVERTISE_CLIENT_URLS="http://10.1.99.3:2379"
ETCD_ENABLE_V2="true"
ETCD_INITIAL_ELECTION_TICK_ADVANCE="false"
```

8. Добавьте пример конфигураций в файл для узла postgres-server3.your\_domain:

```
ETCD_NAME="postgres-server3"
ETCD_DATA_DIR="/var/lib/etcd/default"
ETCD_HEARTBEAT_INTERVAL="1000"
ETCD_ELECTION_TIMEOUT="5000"
ETCD_LISTEN_PEER_URLS="http://10.1.99.4:2380"
ETCD_LISTEN_CLIENT_URLS="http://10.1.99.4:2379,http://127.0.0.1:2379"
ETCD_INITIAL_ADVERTISE_PEER_URLS="http://10.1.99.4:2380"
ETCD_INITIAL_CLUSTER="postgres-server1=http://10.1.99.2:2380,postgres-server2=http://10.1.99.3:2380,postgres-server3=http://10.1.99.4:2380"
ETCD_INITIAL_CLUSTER_STATE="new"
ETCD_INITIAL_CLUSTER_TOKEN="etcd-postgres-cluster"
ETCD_ADVERTISE_CLIENT_URLS="http://10.1.99.4:2379"
ETCD_ENABLE_V2="true"
ETCD_INITIAL_ELECTION_TICK_ADVANCE="false"
```

9. Перезапустите etcd на всех узлах:

```
systemctl restart etcd
```

10. Проверьте состояние кластера.

```
etcdctl cluster-health
```

### Установка и настройка Patroni <a href="#patroni" id="patroni"></a>

1. Остановите PostgreSQL на всех узлах:

```
systemctl stop postgresql
```

2. Удалите каталог данных на ноде postgres-server2.your\_domain и других серверах, используемых в качестве slave:

```
rm -rf /var/lib/postgresql/13/main
```

3. Установите Patroni и PIP на все узлы:

```
apt-get install python3-pip python3-dev libpq-dev -y
apt-get install patroni -y
```

4. Установите зависимости для работы Patroni на все узлы:

```
pip3 install psycopg2-binary==2.8.6
pip3 install wheel
pip3 install python-etcd
```

5. Создайте файл настроек:

```
nano /etc/patroni/config.yml
```

6. В созданный файл /etc/patroni/config.yml поместите пример начальной конфигурации, изменив IP-адреса на свои на каждом узле кластера. Обратите внимание на комментарии в данном файле.

```
scope: postgres-cluster # одинаковое значение на всех узлах
name: postgresql-server1 # разное значение на всех узлах
namespace: /service/ # одинаковое значение на всех узлах

restapi:
  listen: postgres-server1.your_domain:8008 # адрес узла, на котором находится этот файл
  connect_address: postgres-server1.your_domain:8008 # адрес узла, на котором находится этот файл

etcd:
  hosts: postgres-server1.your_domain:2379,postgres-server2.your_domain:2379,postgres-server3.your_domain:2379 # список всех узлов, на которых установлен etcd

bootstrap:
  method: initdb
  dcs:
    ttl: 30
    loop_wait: 10
    retry_timeout: 10
    maximum_lag_on_failover: 1048576
    master_start_timeout: 300
    synchronous_mode: false
    synchronous_mode_strict: false
    synchronous_node_count: 1
    postgresql:
      use_pg_rewind: true
      use_slots: true
      parameters:
        max_connections: 2000
        superuser_reserved_connections: 5
        max_locks_per_transaction: 64
        max_prepared_transactions: 0
        huge_pages: try
        shared_buffers: 512MB
        work_mem: 128MB
        maintenance_work_mem: 256MB
        effective_cache_size: 4GB
        checkpoint_timeout: 15min
        checkpoint_completion_target: 0.9
        wal_compression: on
        min_wal_size: 2GB
        max_wal_size: 4GB
        wal_buffers: 32MB
        default_statistics_target: 1000
        seq_page_cost: 1
        random_page_cost: 4
        effective_io_concurrency: 2
        synchronous_commit: on
        autovacuum: on
        autovacuum_max_workers: 5
        autovacuum_vacuum_scale_factor: 0.01
        autovacuum_analyze_scale_factor: 0.02
        autovacuum_vacuum_cost_limit: 200
        autovacuum_vacuum_cost_delay: 20
        autovacuum_naptime: 1s
        max_files_per_process: 4096
        archive_mode: on
        archive_timeout: 1800s
        archive_command: cd .
        wal_level: replica
        wal_keep_segments: 130
        max_wal_senders: 10
        max_replication_slots: 10
        hot_standby: on
        hot_standby_feedback: True
        wal_log_hints: on
        shared_preload_libraries: pg_stat_statements,auto_explain
        pg_stat_statements.max: 10000
        pg_stat_statements.track: all
        pg_stat_statements.save: off
        auto_explain.log_min_duration: 10s
        auto_explain.log_analyze: true
        auto_explain.log_buffers: true
        auto_explain.log_timing: false
        auto_explain.log_triggers: true
        auto_explain.log_verbose: true
        auto_explain.log_nested_statements: true
        track_io_timing: on
        log_lock_waits: on
        log_temp_files: 3
        track_activities: on
        track_counts: on
        track_functions: all
        log_checkpoints: on
        logging_collector: on
        log_truncate_on_rotation: on
        log_rotation_age: 1d
        log_rotation_size: 0
        log_line_prefix: '%t [%p-%l] %r %q%u@%d '
        log_filename: 'postgresql-%a.log'
        log_directory: /var/log/postgresql

  initdb: # List options to be passed on to initdb
    - encoding: UTF8
    - locale: en_US.UTF-8
    - data-checksums

  pg_hba: # должен содержать адреса ВСЕХ машин, используемых в кластере
    - local all postgres peer
    - local all all peer
    - host all all 0.0.0.0/0 md5
    - host replication replicator localhost trust
    - host replication replicator 10.1.99.0/24 md5

postgresql:
  listen: 10.1.99.2,127.0.0.1:5432 # адрес узла, на котором находится этот файл
  connect_address: 10.1.99.2:5432 # адрес узла, на котором находится этот файл
  use_unix_socket: true
  data_dir: /var/lib/postgresql/13/main # каталог данных
  bin_dir: /usr/lib/postgresql/13/bin
  config_dir: /etc/postgresql/13/main
  pgpass: /var/lib/postgresql/.pgpass_patroni
  authentication:
    replication:
      username: replicator
      password: engine
    superuser:
      username: postgres
      password: engine
  parameters:
    unix_socket_directories: /var/run/postgresql
  pg_hba: # должен содержать адреса ВСЕХ машин, используемых в кластере
    - local all postgres peer
    - local all all peer
    - host all all 0.0.0.0/0 md5
    - host replication replicator localhost trust
    - host replication replicator 10.1.99.0/24 md5

  remove_data_directory_on_rewind_failure: false
  remove_data_directory_on_diverged_timelines: false

  create_replica_methods:
    - basebackup
  basebackup:
    max-rate: '100M'
    checkpoint: 'fast'

watchdog:
  mode: off # Allowed values: off, automatic, required
  device: /dev/watchdog
  safety_margin: 5

tags:
  nofailover: false
  noloadbalance: false
  clonefrom: false
  nosync: false
```

7. Сделайте пользователя postgres владельцем каталога настроек:

```
sudo chown postgres:postgres -R /etc/patroni
sudo chmod 700 /etc/patroni
```

8. Запустите службу postgresql на всех узлах:

```
systemctl enable --now postgresql
```

9. Запустите службу Patroni на узле postgres-server1.your\_domain, а затем на узлах postgres-server2.your\_domain и остальных серверах с ролью slave:

```
sudo systemctl enable --now patroni.service
```

10. Проверьте состояние кластера:

```
patronictl -c /etc/patroni/config.yml list
```

### Настройка балансировки PostgreSQL + HAProxy <a href="#haproxy" id="haproxy"></a>

1. Подключитесь к машине балансировщика и откройте для редактирования конфигурационный файл haproxy.cfg:

```
nano /etc/haproxy/haproxy.cfg
```

2. Добавьте в конец файла базовую конфигурацию postgresql:

<pre><code><strong>### PostgreSQL ###
</strong>listen postgres_master
    bind haproxy-server.your_domain:5000
    mode tcp
    option tcplog
    option httpchk OPTIONS /master
    http-check expect status 200
    default-server inter 3s fastinter 1s fall 3 rise 4 on-marked-down shutdown-sessions
    server postgres-server1 postgres-server1.your_domain:5432 check port 8008
    server postgres-server2 postgres-server2.your_domain:5432 check port 8008
    server postgres-server3 postgres-server3.your_domain:5432 check port 8008

listen postgres_replicas
    bind haproxy-server.your_domain:5001
    mode tcp
    option tcplog
    option httpchk OPTIONS /replica
    balance roundrobin
    http-check expect status 200
    default-server inter 3s fastinter 1s fall 3 rise 2 on-marked-down shutdown-sessions
    server postgres-server1 postgres-server1.your_domain:5432 check port 8008
    server postgres-server2 postgres-server2.your_domain:5432 check port 8008
    server postgres-server3 postgres-server3.your_domain:5432 check port 8008
### PostgreSQL ###
</code></pre>

3. Перезагрузите сервис haproxy:

```
systemctl restart haproxy
```

4. Подключитесь к машине HOSTVM VDI Broker и отредактируйте файл настроек /var/server/server/settings.py, изменив в блоке DATABASES поля HOST и PORT:

```
DATABASES = {
   'default': {
       'ENGINE': 'django.db.backends.postgresql',
       'OPTIONS': {
           'isolation_level': 'read committed',
       },
       'NAME': 'udsdb',                    
       'USER': 'udsdbadm',          
       'PASSWORD': 'engine',   #       
       'HOST': '10.1.99.5',    # Адрес сервера HAProxy         
       'PORT': '5000',         # Порт, указанный в haproxy.cfg                
   }
}
```

5. Перезагрузите службы брокера HOSTVM VDI:

```
systemctl restart vdi.service vdiweb.service
```

При отключении master-сервера роль master\`а автоматически будет назначена одному из slave-серверов.

### Примеры использования Patroni <a href="#examples" id="examples"></a>

Переключение Replica->Master в случае падения мастера:

```
patronictl -c /etc/patroni/config.yml failover

Candidate ['postgresql-server1', 'postgresql-server3'] []: postgresql-server3
Current cluster topology
+ Cluster: postgres-cluster (7286124261676651167)  ---+----+-----------+
| Member             | Host       | Role    | State   | TL | Lag in MB |
+--------------------+------------+---------+---------+----+-----------+
| postgresql-server1 | 10.1.99.2 | Replica | running | 40 |         0  |
| postgresql-server2 | 10.1.99.3 | Leader  | running | 40 |            |
| postgresql-server3 | 10.1.99.4 | Replica | running | 40 |         0  |
+--------------------+------------+---------+---------+----+-----------+
Are you sure you want to failover cluster postgres-cluster, demoting current master postgresql-server2? [y/N]: y
Successfully failed over to "postgresql-server3"
+ Cluster: postgres-cluster (7286124261676651167) ---+----+-----------+
| Member             | Host       | Role    | State   | TL | Lag in MB |
+--------------------+------------+---------+---------+----+-----------+
| postgresql-server1 | 10.1.99.2 | Replica | running | 40 |        11  |
| postgresql-server2 | 10.1.99.3 | Replica | stopped |    |   unknown  |
| postgresql-server3 | 10.1.99.4 | Leader  | running | 40 |            |
+--------------------+------------+---------+---------+----+-----------+
```

Изменение лидера:

```
patronictl -c /etc/patroni/config.yml switchover

Master [postgresql-server3]: postgresql-server3
Candidate ['postgresql-server1', 'postgresql-server2'] []: postgresql-server1
When should the switchover take place [now]: now
Current cluster topology
+ Cluster: postgres-cluster (7286124261676651167)  ---+----+-----------+
| Member             | Host       | Role    | State   | TL | Lag in MB |
+--------------------+------------+---------+---------+----+-----------+
| postgresql-server1 | 10.1.99.2 | Replica | running | 41 |         0  |
| postgresql-server2 | 10.1.99.3 | Replica | running | 41 |         0  |
| postgresql-server3 | 10.1.99.4 | Leader  | running | 41 |            |
+--------------------+------------+---------+---------+----+-----------+
Are you sure you want to switchover cluster postgres-cluster, demoting current master postgresql-server3? [y/N]: y
Successfully switched over to "postgresql-server1"
+ Cluster: postgres-cluster (7286124261676651167)  ---+----+-----------+
| Member             | Host       | Role    | State   | TL | Lag in MB |
+--------------------+------------+---------+---------+----+-----------+
| postgresql-server1 | 10.1.99.2 | Leader  | running | 41 |            |
| postgresql-server2 | 10.1.99.3 | Replica | running | 41 |        15  |
| postgresql-server3 | 10.1.99.4 | Replica | stopped |    |   unknown  |
+--------------------+------------+---------+---------+----+-----------+
```

Просмотр истории переключений:

```
sudo patronictl -c /etc/patroni/config.yml history
```

Отключение автоматического переключения Replica-Master:

```
sudo patronictl -c /etc/patroni/config.yml pause
```

Реинициализация кластера:

```
sudo patronictl -c /etc/patroni/config.yml reinit postgres-cluster
postgres-cluster - задается в настройках patroni (scope: ...)
```

Реинициализация ноды:

```
patronictl -c /etc/patroni/patroni.yml reinit postgres-cluster postgresql-server2
```
