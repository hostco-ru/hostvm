# Обновление Standalone HOSTVM Manager через процедуру восстановления из бэкапа

{% hint style="info" %}
Во время обновления запущенные ВМ могут продолжать работу\
**Портал администрирования во время обновления будет недоступен**
{% endhint %}

{% hint style="info" %}
При обновлении Standalone HOSTVM Manager потребуется временное внешнее хранилище данных, куда будет перемещена резервная копия.
{% endhint %}

1. Подключиться по ssh к Standalone HOSTVM Manager
2. Остановить службу engine:

&#x20;`systemctl stop ovirt-engine`&#x20;

3. Создать резервную копию:&#x20;

`engine-backup --mode=backup --scope=all --file=backup.bck --log=backup.log`&#x20;

4. Скопировать созданную резервную копию на внешнее устройство хранения данных
5. Произвести установку Standalone HOSTVM Manager последней версии, согласно [инструкции](../ustanovka-hostvm-4.5/ustanovka-standalone-hostvm-manager-4.5.md)
6. Подключиться по ssh к установленному менеджеру управления
7. Скопировать файл резервной копии на установленную машину
8. Остановить службу engine:

&#x20;`systemctl stop ovirt-engine`&#x20;

9. Выполнить очистку БД engine: `engine-cleanup`&#x20;
10. Запустить процесс восстановления конфигурации из резервной копии командой:

&#x20;`engine-backup --mode=restore --file=/mnt/backup.bck --log=log_file_name --restore-permissions`&#x20;

11. Запустить команду и ответить на вопросы установщика:&#x20;

`engine-setup --offline`&#x20;

12. По окончании установки проверить доступность веб-портала
