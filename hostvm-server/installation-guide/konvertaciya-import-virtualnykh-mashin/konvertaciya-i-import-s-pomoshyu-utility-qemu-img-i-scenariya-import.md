# Конвертация и импорт с помощью утилиты qemu-img и сценария import

Данный способ подходит для машин с несколькими дисками, либо неподдерживаемыми утилитой virt-v2v.

Требуется [предварительная установка драйверов](drivers-preinstallation.md) virtio. Конфигурация машины задается опциями сценария import-to-hostvm.pl или в HOSTVM Manager WebUI при загрузке из export домена.

#### Конвертация ВМ

\
Скопируйте на ранее подготовленный хост OVA файл с ВМ, распакуйте:&#x20;

`tar -xvf <VMNAME>.ova`

Если диски разбиты на части, вида `vmName-disk1.vmdk.000000000*`, склеить каждый из таких дисков командой:

`cat vmName-disk1.vmdk.* > vmName-disk1.vmdk`

После этого конвертируйте диск в qcow2.

Пример команды для конвертации vmdk диска:

`qemu-img convert -p -f vmdk -O qcow2 /path/to/disk.vmdk /path/to/disk.qcow2`

Пример команды для конвертации vhdx диска:

`qemu-img convert -p -f vhdx -O qcow2 /path/to/disk.vhdx /path/to/disk.qcow2`

#### Подготовка к импорту ВМ

1\. Установить скрипт конвертации на хост HOSTVM, с которого будет выполняться импорт ВМ:

`yum install perl perl-XML-Writer perl-Sys-Guestfs`

2\. Загрузить сценарий импорта import-to-hostvm.pl на хост HOSTVM, с которого будет выполняться импорт ВМ. Загрузка выполняется из личного кабинета[ https://lk.pvhostvm.ru/](https://lk.pvhostvm.ru/). Сценарий import-to-hostvm.pl  расположен в каталоге дистрибутивов в папке HOSTVM/Misc/VM Convert/

3\. Выдать права на выполнение:

`chmod u+x import-to-hostvm.pl`

4\. Загрузить получившийся qcow2 в [Export домен](export-domen.md) HOSTVM:

`export LIBGUESTFS_BACKEND=direct ./import-to-hostvm.pl /path/to/disk.qcow2 /path/to/export_domain`

Пример использования сценария импорта доступен в статье [Конвертация виртуальной машины с ОС AltLinux из VMware в HOSTVM](konvertaciya-virtualnoi-mashiny-s-os-altlinux-iz-vmware-v-hostvm.md).

\
