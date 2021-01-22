# Установка Tunneler appliance

## Развертывание Tunneler версии 2.2

### Требования к конфигурации виртуальной машины

Минимальные требования к ресурсам:

* Hard drive: 10 GB
* Memory: 1 G
* CPU: 2 vCPU
* Network: 1 vNIC

### Импорт виртуальной машины

Скачать образ машины UDS-Tuneler-2.2.zip из каталога загрузок HOSTVM. 

Загрузить данный файл на один из хостов виртуализации к HOSTVM с помощью scp или других средств. 

Распаковывать и подготовить к импорту, загрузить в export домен платформы виртуализации:

```bash
unzip UDS-Tuneler-2.2.zip
mv UDS-Tuneler-2.2 UDS-Tuneler-2.2.qcow2
qemu-img convert ./UDS-Tuneler-2.2.qcow2 -O raw ./UDS-Tuneler-2.2.raw
export LIBGUESTFS_BACKEND=direct
virt-v2v -i disk /home/UDS-Tuneler-2.2.raw -o rhv -os /export/domain/mount/point
```

Точку монтирования домена можно посмотреть через веб-интерфейс платформы виртуализации:

 `GUI>Storage>Domains>выбрать export домен>Manage domain>путь будет указан в поле Path`

Импортировать через `GUI>compute>virtual machines>import`, в качестве источника выбрать export domain. При импорте необходимо добавить сеть \(перейти на вкладку **Network Interfaces** и указать нужную сеть для данной ВМ\).

### Настройка

Для настройки нужно подключиться к ВМ через spice консоль: `GUI>compute>virtual machines>выбрать машину>нажать кнопку Console`

Первый запуск конфигуратора нужно отменить, затем добавить сертификат машины HOSTVM VDI в доверенные, иначе настройка завершится ошибкой.

```bash
cp cert-bundle.crt /usr/local/share/ca-certificates/
update-ca-certificates
```

После настройки сертификатов запустить конфигуратор повторно командой `SetupTunneler.sh`

Дальнейший процесс установки проходит в графическом режиме, необходимо указать следующие параметры:

```text
Network configuration:
hostname: имя машины туннелера
domain: домен машины
ip: IP-адрес
mask: маска сети
gateway: шлюз
primary dns: IP-адрес сервера DNS
secondary dns: IP-адрес сервера DNS

Broker configuration:
broker server url: ссылка на веб-интерфейс машины HOSTVM VDI, например https://vdi.domain.ru
```

### Настройка транспорта Spice tunnel в панели управления HOSTVM VDI

При настройке транспорта Spice tunnel в панели управления HOSTVM VDI, в качестве tunnel server указывается `имя_машины_туннелера:443`

## Развертывание Tunneler версии 3.0

### Требования к конфигурации виртуальной машины

* Hard drive: 10 GB
* Memory: 2 G
* CPU: 2 vCPU
* Network: 1 vNIC

### Импорт виртуальной машины

Скачать образ машины UDS-Tunnel-qcow2.3.0.0.zip из каталога загрузок HOSTVM. 

Загрузить данный файл на один из хостов виртуализации к HOSTVM с помощью scp или других средств. 

Распаковывать и подготовить к импорту, загрузить в export домен платформы виртуализации:

```bash
unzip UDS-Tunnel-qcow2.3.0.0.zip
qemu-img convert ./UDS-Tunnel-qcow2.3.0.0 -O raw ./UDS-Tuneler-3.0.raw
export LIBGUESTFS_BACKEND=direct
virt-v2v -i disk /home/UDS-Tuneler-3.0.raw -o rhv -os /export/domain/mount/point
```

Точку монтирования домена можно посмотреть через веб-интерфейс платформы виртуализации:

 `GUI>Storage>Domains>выбрать export домен>Manage domain>путь будет указан в поле Path`

Импортировать через `GUI>compute>virtual machines>import`, в качестве источника выбрать export domain. При импорте необходимо добавить сеть \(перейти на вкладку **Network Interfaces** и указать нужную сеть для данной ВМ\).

### Настройка

Для настройки нужно подключиться к ВМ через spice консоль: `GUI>compute>virtual machines>выбрать машину>нажать кнопку Console`

После подключения отредактировать раскладку в `/etc/default/keyboard`:

```text
    XKBLAYOUT="us"
```

Применить изменения командой:

`setupcon`

Настроить сеть, если нет dhcp \(без сети конфигуратор не запустится\). Пример настройки:

`/etc/network/interfaces`

```text
    allow-hotplug eth0
    iface eth0 inet static
    address 10.1.1.2
    netmask 255.255.255.0
    gateway 10.1.1.1
```

Указать адреса DNS в `/etc/resolv.conf`

Перезапустить интерфейс для применения настроек.

```bash
ifdown eth0
ifup eth0
```

Перед запуском конфигуратора нужно установить сертификат машины HOSTVM VDI в доверенные, иначе настройка завершится ошибкой.

```bash
cp cert-bundle.crt /usr/local/share/ca-certificates/
update-ca-certificates
```

Дальнейшая настройка производится через браузер по адресу: `<tunneler_ip>:9900`

### Настройка транспорта Spice tunnel в панели управления HOSTVM VDI

При настройке транспорта Spice tunnel в панели управления HOSTVM VDI, в качестве tunnel server указывается `имя_машины_туннелера:443`

