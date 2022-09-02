# Конвертация ВМ из Microsoft Hyper-V

### Конвертация и импорт с помощью утилиты virt-v2v

Данный способ подходит для машин с одним диском, и является рекомендуемым, т.к. сразу определяется текущая конфигурация и встраиваются необходимые драйверы.

[Список поддерживаемых утилитой гостевых ОС](https://libguestfs.org/virt-v2v-support.1.html)

Пример использования утилиты, для конвертации и загрузки в Export домен HOSTVM:

```
virt-v2v -i disk /path/to/disk.vhdx -o rhv -of qcow2 -os /path/to/export_domain
```

[Полное описание и перечень опций](https://libguestfs.org/virt-v2v.1.html)

### Конвертация и импорт с помощью утилиты qemu-img и сценария import <a href="#user-content-konvertaciya-i-import-s-pomoshyu-utility-qemu-img-i-skripta-import-to-ovirt" id="user-content-konvertaciya-i-import-s-pomoshyu-utility-qemu-img-i-skripta-import-to-ovirt"></a>

Данный способ подходит для машин с несколькими дисками, либо неподдерживаемыми утилитой virt-v2v.

Требуется [предварительная установка драйверов](drivers-preinstallation.md) virtio. Конфигурация машины задается опциями сценария import-to-ovirt.pl или в HOSTVM Manager WebUI при загрузке из export домена.

Загрузить файл сценария конвертации import-to-ovirt.pl на хост HOSTVM, с которого будет выполняться импорт ВМ. Загрузка выполняется из личного кабинета [https://lk.pvhostvm.ru/](https://lk.pvhostvm.ru/) Сценарий import-to-ovirt.pl  расположен в каталоге дистрибутивов в папке HOSTVM/VM Convert/

Пример команды для конвертации диска:

```
qemu-img convert -p -f vhdx -O qcow2 /path/to/disk.vhdx /path/to/disk.qcow2
```

и загрузки получившегося qcow2 в Export домен HOSTVM:

```
export LIBGUESTFS_BACKEND=direct
./import-to-ovirt.pl /path/to/disk.qcow2 /path/to/export_domain
```

### Конвертация дисков с помощью StarWind V2V Converter <a href="#user-content-konvertaciya-problemnykh-diskov-s-pomoshyu-po-starwind" id="user-content-konvertaciya-problemnykh-diskov-s-pomoshyu-po-starwind"></a>

Данный способ конвертации также поддерживается для импорта виртуальных машин в HOSTVM.&#x20;

Потребуется отдельная Windows машина с установленным ПО StarWind V2V Converter.

Порядок действий:

* Скопировать vhdx на Windows машину;
* Запустить ПО Starwind;
* Выбрать local file, указать на конверируемый жесткий диск;
* Выбрать формат результирующего диска qcow2, далее снова выбрать local file и указать место сохранения результирующего диска;
* Выгрузить получившийся qcow2 диск.

Загрузка в Export домен HOSTVM производится с помощью скрипта import-to-ovirt.pl Загрузить файл сценария конвертации import-to-ovirt.pl на хост HOSTVM, с которого будет выполняться импорт ВМ. Загрузка выполняется из личного кабинета [https://lk.pvhostvm.ru/](https://lk.pvhostvm.ru/) Сценарий import-to-ovirt.pl  расположен в каталоге дистрибутивов в папке HOSTVM/VM Convert/

```
export LIBGUESTFS_BACKEND=direct
./import-to-ovirt.pl /path/to/disk.qcow2 /path/to/export_domain
```
