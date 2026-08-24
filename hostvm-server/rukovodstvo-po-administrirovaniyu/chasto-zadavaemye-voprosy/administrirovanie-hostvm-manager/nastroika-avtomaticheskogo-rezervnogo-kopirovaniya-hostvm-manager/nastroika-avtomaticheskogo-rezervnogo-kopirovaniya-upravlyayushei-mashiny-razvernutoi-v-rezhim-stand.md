---
description: >-
  Инструкция описывает настройку автоматического резервного копирования
  управляющей машины HOSTVM Manager, развернутой в режиме Standalone.
---

# Настройка автоматического резервного копирования управляющей машины развернутой в режим Standalone

В режиме Standalone для запуска резервного копирования не требуется перевод среды в глобальный режим обслуживания.

Во время выполнения резервного копирования портал управления HOSTVM Manager будет недоступен. Продолжительность простоя зависит от размера базы данных, конфигурации Manager и скорости дисковой подсистемы.

### Размещение скрипта и подготовка окружения

1. Загрузить скрипт backup\_standalone\_manager.sh из директории HOSTVM/Misc/Backup/Standalone  и скопировать его на управляющую  машину (например, с помощью WinSCP или SCP)
2. При необходимости измените следующие параметры:

```
#Место хранения логов по умолчанию
BACKUP_DIR="/var/lib/hostvm-manager-backup" 

#Период хранения логов по умолчанию 14 дней
RETENTION_DAYS=14 
```

3. Сделайте скрипт исполняемым:

```
chmod +x /usr/local/bin/backup_standalone_manager.sh
```

4. Создайте каталог для хранения резервных копий

```
mkdir -p /var/lib/hostvm-manager-backup
```

5. Установите права на каталог для размещения резервных копий и логов:

<pre><code>chown root:root /var/lib/hostvm-manager-backup
<strong>chmod 750 /var/lib/hostvm-manager-backup
</strong></code></pre>

6. Проверьте наличие свободного места:

```
df -h /var/lib/hostvm-manager-backup
```

{% hint style="warning" %}
В выбранной директории должно быть достаточно свободного места для хранения резервных копий.
{% endhint %}

### Тестирование скрипта (ручной запуск)

Перед настройкой автоматического запуска выполните резервное копирование вручную.

1. Запустите скрипт

```
/usr/local/bin/backup_standalone_manager.sh
```

2. Проверьте результат (обратите внимание на дату в имени файла)

```
ls -la /var/lib/hostvm-manager-backup/
```

```
Пример вывода:
# backup_05072026.bck
# backup_05072026.log
```

3. Проверьте содержимое лога на наличие ошибок

```
cat /var/lib/hostvm-manager-backup/backup_05072026.log
```

4. Убедитесь, что сервис управления работает

```
systemctl status ovirt-engine
```

Если скрипт завершился с ошибкой, проверьте права доступа и доступное место на диске, прежде чем переходить к настройке автоматического запуска.

### Настройка автоматического запуска

Для запуска резервного копирования по расписанию используются:

* `service` — описание операции резервного копирования;
* `timer` — расписание запуска службы.

#### 1. Создание service-файла

Создайте файл `/etc/systemd/system/``backup_standalone_manager.service`&#x20;

```
vi /etc/systemd/system/backup_standalone_manager.service
```

Следующего содержания

```
[Unit]
Description=HOSTVM Standalone Manager Backup
After=network.target
ConditionPathExists=/usr/local/bin/backup_standalone_manager.sh

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup_standalone_manager.sh
User=root
StandardOutput=journal
StandardError=journal
```

#### 2. Создание timer-файла

Создайте файл `/etc/systemd/system/backup_standalone_manager.timer`&#x20;

```
vi /etc/systemd/system/backup_standalone_manager.timer 
```

Следующего содержания

```
ini
[Unit]
Description=Run HOSTVM Manager Backup daily at 2 AM

[Timer]
OnCalendar=Mon..Sun 02:00
Persistent=true
RandomizedDelaySec=300
Unit=backup_standalone_manager.service

[Install]
WantedBy=timers.target
```

Данная конфигурация задает ежедневный запуск резервного копирования около 02:00.

`RandomizedDelaySec=300` добавляет случайную задержку до пяти минут. Это позволяет избежать одновременного запуска нескольких ресурсоемких заданий, если на сервере или в инфраструктуре настроены другие задачи резервного копирования.

`Persistent=true` позволяет выполнить пропущенный запуск после перезагрузки управляющей машины.

#### 3. Активация таймера

Перечитайте конфигурацию systemd

```
systemctl daemon-reload
```

Включите таймер при загрузке&#x20;

```
systemctl enable --now backup_standalone_manager.timer
```

Активируйте таймер

```
systemctl start backup_standalone_manager.timer
```

Проверьте состояние таймера

```
systemctl status backup_standalone_manager.timer
```

Проверьте наличие таймера в общем списке

```
systemctl list-timers --all | grep backup_standalone_manager          
```

Для немедленного тестирования службы без ожидания расписания выполните

```
systemctl start backup_standalone_manager.service
```

Проверьте результат

```
systemctl status backup_standalone_manager.service
```

Просмотрите журнал службы

```
journalctl -u backup_standalone_manager.service -n 100 --no-pager
```

#### 4. Мониторинг резервного копирования и логов

Базовые команды для быстрой проверки состояния бэкапов:

```
# Просмотр последних записей в логе (в реальном времени)
tail -f /var/lib/hostvm-manager-backup/backup_*.log

# Просмотр последних 50 строк лога
tail -n 50 /var/lib/hostvm-manager-backup/backup_*.log

# Проверка размера и даты создания последнего бэкапа
ls -lah /var/lib/hostvm-manager-backup/ | grep .bck

# Проверка свободного места в разделе с бэкапами
df -h /var/lib/hostvm-manager-backup/
```
