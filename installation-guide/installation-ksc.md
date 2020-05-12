# Установка Kaspersky Security для виртуальных сред 5.х Легкий агент на систему управления виртуализац

## Общие данные

Данный документ представляет собой руководство по установке пакета ПО Kaspersky Security для виртуальных сред Легкий агент в систему управления виртуализацией HOSTVM.

## Инструкция по установке

### Требования к конфигурации машины

Требования к конфигурации машины:

* процессор: 8 ядер, 2,5 ГГц, 2:2:1;
* объем ОП: 16 ГБ. 8;
* жесткий диск: 500 ГБ SATA RAID. 40;
* сетевой адаптер: 1 Гбит. 1;
* БД: Microsoft SQL Server 2008 R2 Express \([https://www.microsoft.com/ru-ru/download/details.aspx?id=30438](https://www.microsoft.com/ru-ru/download/details.aspx?id=30438)\).

  **Установка компонентов легкого агента на KSC**

Для установки компонентов необходимо:

1. Запустить файл `ksvla-components_5.1.0.405_mlg.exe;`
2. Выполнить рестарт консоли KSC;
3. После запуска мастеров настройки компонентов легкого агента выбрать все значения по умолчанию;
4. Подготовить ноду HOSTVM. Для этого в консоли ОС гипервизора выполнить команды:

   `virsh -c qemu:///system version`

   `disable libvirt auth in /etc/libvirt/libvirtd.conf`

   `## beginning of configuration section by vdsm-4.17.0`

   `auth_unix_rw="sasl" поменять на auth_unix_rw="none"`

   `service libvirtd restart'`

### Создание пула хранения для SVM

Для создания пула хранения необходимо:

1. Создать директорию для хранилища;

   `mkdir /images`

   `chown root:kvm /image`

   `chmod 750 /images`

2. Cоздать представление пула хранения данных;

   `virsh pool-define-as images dir - - - - "/images"`

3. Создать новое хранилище на основе каталога;

   `virsh pool-build images`

4. Запустить хранилище;

   `virsh pool-start images`

5. Включить автозапуск для хранилища данных;

   `virsh pool-autostart images`

6. Убедиться, что хранилище настроено правильно:

   `virsh pool-info images`

### Настройка машины с KSC

После распаковки ВМ до ее запуска необходимо:

1. Выполнить команду в консоли ОС гипервизора:

   `chown qemu:qemu /images/ksvla-svm...`

2. Далее выполнить команды \(т.к. ВМ запускается как `transient domain`\):

   `virsh dumpxml <vmname> > ./<vmname>.xml`

   `virsh shutdown <vmname>`

   `virsh./<vmname>.xml`

   `edit clock from 'localtime' to 'utc'`

   `virsh define ./<vmname>.xml`

   `virsh start <vmname>`

3. Установить правильную таймзону:

   `timedatectl set-timezone Asia/Yekaterinburg`

4. Внести имя машины в DNS либо в локальный файл `/etc/hosts` на SVM \(т.к. имя машины KSC должно разрешаться в IP-адрес при обращении к нему с SVM\);
5. Отключить брандмауэр на машине KSC;
6. Настроить и применить политику для SVM, а также указать в ней подключение к серверу интеграции.

