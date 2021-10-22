# Конвертация виртуальных машин из Microsoft Hyper-V



### Конвертация и импорт с помощью утилиты virt-v2v

Данный способ подходит для машин с одним диском, и является предпочтительным, т.к. сразу определяется текущая конфигурация и встраиваются необходимые драйверы.

[Список поддерживаемых утилитой гостевых ОС](https://libguestfs.org/virt-v2v-support.1.html)

Пример использования утилиты, для конвертации и загрузки в Export домен HOSTVM:

```
virt-v2v -i disk /path/to/disk.vhdx -o rhv -of qcow2 -os /path/to/export_domain
```

[Полное описание и перечень опций](https://libguestfs.org/virt-v2v.1.html)

### Конвертация и импорт с помощью утилиты qemu-img и скрипта import <a href="user-content-konvertaciya-i-import-s-pomoshyu-utility-qemu-img-i-skripta-import-to-ovirt" id="user-content-konvertaciya-i-import-s-pomoshyu-utility-qemu-img-i-skripta-import-to-ovirt"></a>

Данный способ подходит для машин с несколькими дисками, либо неподдерживаемыми утилитой virt-v2v.

Требуется предварительная установка драйверов virtio. Конфигурация машины задается опциями скрипта import-to-ovirt.pl (скрипт доступен для загрузки в личном кабинете HOSTVM) или в HOSTVM engine webUI при загрузке из export домена.

Пример команды для конвертации диска:

```
qemu-img convert -p -f vhdx -O qcow2 /path/to/disk.vhdx /path/to/disk.qcow2
```

и загрузки получившегося qcow2 в Export домен HOSTVM:

```
export LIBGUESTFS_BACKEND=direct
./import-to-ovirt.pl /path/to/disk.qcow2 /path/to/export_domain
```

### Конвертация проблемных дисков с помощью ПО Starwind <a href="user-content-konvertaciya-problemnykh-diskov-s-pomoshyu-po-starwind" id="user-content-konvertaciya-problemnykh-diskov-s-pomoshyu-po-starwind"></a>

Данный способ подходит для дисков машин, которые не конвертируются вышеуказанными способами. Требуется отдельная Windows машина с установленным ПО Starwind.

Порядок действий:

* Скопировать vhdx на Windows машину;
* Запустить ПО Starwind;
* Выбрать local file, указать на конверируемый жесткий диск;
* Выбрать формат результирующего диска qcow2, далее снова выбрать local file и указать место сохранения результирующего диска;
* Выгрузить получившийся qcow2 диск.

Загрузка в Export домен HOSTVM производится с помощью скрипта import-to-ovirt:

```
export LIBGUESTFS_BACKEND=direct
./import-to-ovirt.pl /path/to/disk.qcow2 /path/to/export_domain
```
