---
description: >-
  Инструкция описывает настройку автоматического резервного копирования
  управляющей виртуальной машины HOSTVM Manager, развернутой в режиме
  Self-Hosted.
---

# Настройка автоматического резервного копирования управляющей машины развернутой в режим Self-Hosted

Скрипт выполняет следующие действия:

1. Определяет доступный хост с ролью `Self-Hosted`.
2. Переводит среду Self-Hosted в глобальный режим обслуживания.
3. Останавливает сервис управления на управляющей машине.
4. Создает полную резервную копию HOSTVM Manager.
5. Запускает сервис управления на управляющей машине.
6. Выводит среду Self-Hosted из режима обслуживания.
7. Удаляет резервные копии и журналы, срок хранения которых истек.

Глобальный режим обслуживания предотвращает автоматическую миграцию или запуск управляющей ВМ другим узлом во время резервного копирования. Для создания согласованной резервной копии базу данных и файлы Manager рекомендуется резервировать при остановленной службе `ovirt-engine`.

{% hint style="warning" %}
Во время выполнения резервного копирования портал управления HOSTVM Manager будет недоступен. Продолжительность простоя зависит от размера базы данных, конфигурации Manager и скорости дисковой подсистемы.
{% endhint %}

### Размещение скрипта и подготовка окружения

1. Загрузить скрипт backup\_self-hosted\_manager.sh из директории HOSTVM/Misc/Backup/Self-Hosted и скопировать его на управляющую машину (например, с помощью WinSCP или SCP)
2. Отредактируйте скрипт, изменив следующие параметры на необходимые:

```
vi /usr/local/bin/backup_self-hosted_manager.sh
```

* HOSTS - список хостов с ролью self-hosted
* BACKUP\_DIR - директория для хранения бэкапов
* RETENTION\_DAY - срок хранения бэкапов

Пример конфигурации:

```
# Список хостов кластера (измените на свои)
HOSTS=(
    "hostvm-node1.local"
    "hostvm-node2.local"
    "hostvm-node3.local"
)

# Место хранения бэкапов по умолчанию
BACKUP_DIR="/var/lib/hostvm-manager-backup"

# Период хранения бэкапов по умолчанию 14 дней
RETENTION_DAYS=14
```

{% hint style="warning" %}
В массив `HOSTS` необходимо включать только те хосты, на которых разрешена роль Self-Hosted.
{% endhint %}

3. Сделайте скрипт исполняемым:

```
chmod +x /usr/local/bin/backup_self-hosted_manager.sh
```

4. Создайте каталог для хранения резервных копий

```
mkdir -p /var/lib/hostvm-manager-backup
```

* Установите права на каталог для размещения резервных копий и логов:

```
chown root:root /var/lib/hostvm-manager-backup
chmod 750 /var/lib/hostvm-manager-backup
```

5. Проверьте наличие свободного места:

```
df -h /var/lib/hostvm-manager-backup
```

{% hint style="warning" %}
В выбранной директории должно быть достаточно свободного места для хранения резервных копий.
{% endhint %}

### Настройка SSH-доступа

Для работы скрипта необходим беспарольный SSH-доступ от управляющей машины ко всем хостам из массива `HOSTS`.

Скрипт ищет SSH-ключ в следующем порядке:

```
1. /root/.ssh/hostvm_backup_self_hosted_id_rsa
2. /root/.ssh/id_rsa_ovirt
3. /root/.ssh/id_rsa
4. /root/.ssh/id_ed25519
5. /root/.ssh/id_ecdsa
```

1. Создайте SSH-ключ с именем `hostvm_backup_self_hosted_id_rsa`

```
ssh-keygen -t rsa -b 4096 -f ~/.ssh/hostvm_backup_self_hosted_id_rsa -C "backup_hostvm-self-hosted-key"
```

Фразу-пароль для ключа оставьте пустой, если скрипт должен запускаться автоматически без участия администратора. Если политика безопасности требует пароль для ключа, необходимо использовать отдельное решение для безопасной автоматизации, например `ssh-agent` или специализированное хранилище секретов.

2. Выполните копирование ключа на все хосты из списка

```
# Для каждого хоста из списка
for host in \
    hostvm-node1.local \ 
    hostvm-node2.local  \
    hostvm-node3.local; 
do
    echo "Копирование ключа на $host"
    ssh-copy-id -i ~/.ssh/hostvm_backup_self_hosted_id_rsa.pub root@$host
done
```

При запросе введите пароль пользователя `root` соответствующего хоста.

3. Проверьте, что подключение работает корректно ко всем хостам из массива `HOSTS`. Для каждого хоста должно отобразиться его имя:

```
for host in \
  hostvm-node1.local \
  hostvm-node2.local \
  hostvm-node3.local
do
    echo "Проверка ${host}:"
    ssh -o BatchMode=yes \
      -o ConnectTimeout=10 \
      -i /root/.ssh/hostvm_backup_self_hosted_id_rsa \
      root@"${host}" hostname
done
```

Для проверки отдельного хоста

```
ssh -o BatchMode=yes \
  -o ConnectTimeout=10 \
  -i /root/.ssh/hostvm_backup_self_hosted_id_rsa \
  root@hostvm-node1.local hostname
```

В ответ должно отобразиться имя хоста. Если подключение не выполняется, проверьте:

* сетевую доступность хоста;
* разрешение DNS-имени;
* наличие открытого SSH-порта;
* содержимое файла `/root/.ssh/authorized_keys` на хосте;
* права на каталог `/root/.ssh`;
* права на закрытый ключ.

### Тестирование скрипта (ручной запуск)

Перед включением автоматического запуска выполните резервное копирование вручную.

1. Запустите скрипт

```
/usr/local/bin/backup_self-hosted_manager.sh
```

2. Проверьте результат (обратите внимание на дату в имени файла)

```
ls -la /var/lib/hostvm-manager-backup/
```

```
Пример вывода:
# backup_15082026.bck
# backup_15082026.log
```

3. Проверьте содержимое лога на наличие ошибок

```
cat /var/lib/hostvm-manager-backup/backup_15082026.log
```

4. Убедитесь, что сервис управления работает

```
systemctl status ovirt-engine
```

5. На любом хосте из списка проверьте, что среда Self-Hosted вышла из глобального режима обслуживания

```
hosted-engine --vm-status
```

Если скрипт завершился с ошибкой, проверьте: наличие SSH-ключей и доступность хостов, права доступа и доступное место на диске и работоспособность команды `hosted-engine --vm-status` на каждом хосте, прежде чем переходить к настройке автоматического запуска

### Настройка автоматического запуска

Для запуска по расписанию используются два файла:

* `service` — описывает саму операцию резервного копирования;
* `timer` — задает расписание запуска.

#### 1. Создание service-файла

Создайте файл `/etc/systemd/system/backup_self-hosted_manager.service`&#x20;

```
vi /etc/systemd/system/backup_self-hosted_manager.service
```

Следующего содержания

```
[Unit]
Description=HOSTVM Self-Hosted Manager Backup
After=network.target
ConditionPathExists=/usr/local/bin/backup_self-hosted_manager.sh

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup_self-hosted_manager.sh
User=root
StandardOutput=journal
StandardError=journal
```

#### 2. Создание timer-файла

Создайте файл `/etc/systemd/system/backup_self-hosted_manager.timer`

```
vi /etc/systemd/system/backup_self-hosted_manager.timer
```

Следующего содержания

```
[Unit]
Description=Run HOSTVM self-hosted backup daily at 2 AM

[Timer]
OnCalendar=Mon..Sun 02:00
Persistent=true
RandomizedDelaySec=300
Unit=backup_self-hosted_manager.service

[Install]
WantedBy=timers.target
```

Эта конфигурация запускает резервное копирование ежедневно около 02:00. Параметр `RandomizedDelaySec=300` добавляет случайную задержку до пяти минут.

Параметр `Persistent=true` позволяет выполнить пропущенный запуск после перезагрузки, если запланированное время было пропущено.

#### 3. Активация таймера

Перечитайте конфигурацию systemd

```
systemctl daemon-reload
```

Включите таймер при загрузке

```
systemctl enable --now backup_self-hosted_manager.timer
```

Активируйте таймер

```
systemctl start backup_self-hosted_manager.timer
```

Проверьте состояние таймера

```
systemctl status backup_self-hosted_manager.timer
```

Проверьте наличие таймера в общем списке

```
systemctl list-timers | grep backup_self-hosted_manager 
```

Для немедленного тестирования службы без ожидания расписания выполните

```
systemctl start backup_self-hosted_manager.service
```

Проверьте результат

```
systemctl status backup_self-hosted_manager.service
```

Просмотрите журнал службы

```
journalctl -u backup_self-hosted_manager.service -n 100 --no-pager
```

#### 4. Мониторинг резервного копирования и логов

Базовые команды для быстрой проверки состояния бэкапов:

<pre><code>#Просмотр логов в реальном времени
tail -f /var/lib/hostvm-manager-backup/backup_*.log
<strong>
</strong><strong>#Просмотр последних 50 строк
</strong>tail -n 50 /var/lib/hostvm-manager-backup/backup_*.log

#Проверка размера и даты последнего бэкапа
ls -lah /var/lib/hostvm-manager-backup/ | grep .bck

#Проверка свободного места
df -h /var/lib/hostvm-manager-backup/

#Ручное управление режимом обслуживания
#Если необходимо вручную включить/выключить режим обслуживания (например, для отладки):
# Включить
hosted-engine --set-maintenance --mode=global
# Выключить
hosted-engine --set-maintenance --mode=none

# Проверить статус
hosted-engine --vm-status | grep -i maintenance

#Проверьте результат
systemctl status backup_self-hosted_manager.service
journalctl -u backup_self-hosted_manager.service -n 100 --no-pager

#Просмотр журналов службы
journalctl -u backup_self-hosted_manager.service -f
</code></pre>
