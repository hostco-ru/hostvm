# Конфигурация подключения Zabbix

Для интеграции Zabbix с HOSTVM воспользуйтесь готовым шаблоном.

Шаблон доступен для скачивания в личном кабинете.

### Установка Zabbix

1\. Установите базовую конфигурацию Zabbix с помощью инструкции с сайта разработчика: [https://www.zabbix.com/ru/download?zabbix=6.2\&os\_distribution=ubuntu\&os\_version=22.04\&components=server\_frontend\_agent\&db=pgsql\&ws=nginx](https://www.zabbix.com/ru/download?zabbix=6.2\&os\_distribution=ubuntu\&os\_version=22.04\&components=server\_frontend\_agent\&db=pgsql\&ws=nginx)

### Инструкция по конфигурации подключения

Зависимости на сервере ovirt engine: jq&#x20;

```
yum install jq
```

1\. Установить zabbix-agent на ovirt engine

1.1. Задать permissive SE linux режим с командой:

```
semanage permissive -a zabbix_agent_t
```

1.2. Задать в конфигурации агента параметры server, hostname, timeout=30

1.3. Скопировать файл zbx-ovirt.conf на сервере ovengine в директорию nclude=/etc/zabbix/zabbix\_agentd.d/ (или другое место), подключить его в конфигурацию агента (Include=/etc/zabbix/zabbix\_agentd.d/\*.conf)

1.4. Скопировать файл zbx-ovirt.sh на сервере ovengine в директорию /etc/zabbix/scripts (или другое место), указать правильный путь в конфигурации агента для UserParameter до данного скрипта

1.5. В скрипте zbx-ovirt.sh задать URL для доступа к API, логин-пароль для подключения.

2\. Установить права на файл zbx-ovirt.sh - 755

3\. Создать хост (ovengine) в zabbix

4\. Присоединить шаблон 'Template oVirt Engine' к хосту (ovengine)

Через 1 час в zabbix создадутся объекты ovirt - hosts, vms, storage domains (При необходимости, заходим в Discovery Rule и выполняем проверку вручную)

Примечание: в конфигурации сервера, timeout необходимо увеличить до 30 сек.
