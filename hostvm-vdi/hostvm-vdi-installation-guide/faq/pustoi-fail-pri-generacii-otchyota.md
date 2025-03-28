# Пустой файл при генерации отчёта

Следующие отчёты генерируются только для сервис-пулов с менеджером ОС:

* Usage by users summary;
* Pools usage by users;
* Summary of Pools usage.

Для провайдера RDS доступны следующие отчёты по пулам: &#x20;

* Pools performance by date;
* Pools usage on a day.

Если необходимо, можно получить аналог отчёта Pools usage by users для сервисов RDS напрямую из БД. Для этого подключитесь к серверу БД и выполните:

1. MariaDB&#x20;

```
SELECT * FROM ( SELECT FROM_UNIXTIME(t1.stamp) AS Timestamp, t2.name AS ServicePool, t1.fld1 as Username, t1.fld2 as user_ip FROM uds_stats_e t1 INNER JOIN uds__deployed_service t2 ON
t1.owner_id = t2.id WHERE t1.event_type = 2 ORDER BY t1.stamp ) AS Report WHERE ServicePool = 'Notepad 1' AND ( Timestamp BETWEEN '2025-02-28 00:00:00' AND '2025-03-26 00:00:00' );
```

`( Timestamp BETWEEN '2025-02-28 00:00:00' AND '2025-03-26 00:00:00' )` - временной промежуток, указанный в формате 'YYYY-MM-DD hh:mm:ss'.

`ServicePool = 'Notepad 1'` - имя сервис-пула, для которого необходимо получить данные.

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

2. PostgreSQL

```
SELECT * FROM ( SELECT TO_TIMESTAMP(t1.stamp) AS Timestamp, t2.name AS ServicePool, t1.fld1 as Username, t1.fld2 as user_ip FROM uds_stats_e t1 INNER JOIN uds__deployed_service t2 ON
t1.owner_id = t2.id WHERE t1.event_type = 2 ORDER BY t1.stamp ) AS Report WHERE ServicePool = 'Windows10' AND ( Timestamp BETWEEN '2025-02-28 00:00:00' AND '2025-03-26 00:00:00' );
```

`( Timestamp BETWEEN '2025-02-28 00:00:00' AND '2025-03-26 00:00:00' )` -  временной промежуток, указанный в формате 'YYYY-MM-DD hh:mm:ss'.

`ServicePool = 'Windows10'` - имя сервис-пула, для которого необходимо получить данные.

<figure><img src="../../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
