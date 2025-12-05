# Конвертация виртуальной машины с ОС AltLinux из VMware в HOSTVM

## Подготовка ВМ к конвертации

1\. Установить гостевые утилиты:

`apt-get install ovirt-guest-agent`

2\. Добавить модули virtio в конфигурационный файл `/etc/initrd.mk`:

`MODULES_ADD += virtio_blk virtio_scsi virtio_pci`

`MODULES_PRELOAD += virtio_blk virtio_scsi virtio_pci`

3\. Сгенерировать новый initramfs:

`make-initrd`

## Экспорт

1\. Экспортировать ВМ из Vmware:

File - Export - Export OVF Template

Format: Single file (OVA)

## Подготовка к импорту ВМ

1\. Установить скрипт конвертации на хост HOSTVM, с которого будет выполняться импорт ВМ:

`yum install perl perl-XML-Writer perl-Sys-Guestfs`

2\. Загрузить сценарий конвертации import-to-hostvm.pl на хост HOSTVM, с которого будет выполняться импорт ВМ. Загрузка выполняется из личного кабинета [https://lk.pvhostvm.ru/](https://lk.pvhostvm.ru/) Сценарий import-to-hostvm.pl  расположен в каталоге дистрибутивов в папке HOSTVM/Misc/VM Convert/

`chmod u+x import-to-hostvm.pl`

## Импорт ВМ

1 Скопировать на ранее подготовленный хост OVA файл с ВМ, распаковать:

`tar -xvf <VMNAME>.ova`

2 Если диски разбиты на части, вида `vmName-disk1.vmdk.000000000*`, склеить каждый из таких дисков командой:

`cat vmName-disk1.vmdk.* > vmName-disk1.vmdk`

3 Сконвертировать ВМ и загрузить в HOSTVM командами:

`export LIBGUESTFS_BACKEND=direct`

`import-to-ovirt.pl --memory <MB> --name <VMNAME> --vcpus <VCPU> --vmtype <TYPE> <disk1name>.vmdk <disk2name>.vmdk <path_to_export_domain>`

где:

`<MB>` - количество оперативной памяти в Мб

`<VMNAME>` - желаемое имя ВМ в HOSTVM

`<VCPU_count>` - количество VCPU для ВМ

`<TYPE>` - одно из двух значений: Server или Desktop

`<disk1name>.vmdk <disk2name>.vmdk` – путь до диска ВМ, если их несколько – указываются через пробел

`<path_to_export_domain>` - путь к [экспорт-домену](export-domen.md) HOSTVM, в виде server:/path/to/esd

(посмотреть путь можно в `engine GUI - Storage - Domains - выбрать export домен - Manage domain` - путь будет указан в поле Path)

либо указать путь до точки монтирования в виде `/mount/point`, если он уже примонтирован на хосте

4 Импортировать ВМ в data домен через GUI:

`HOSTVM Manager GUI - Compute - Virtual machines` - нажать 3 точки в меню - Import - выбрать нужный дата центр, source=export domain - нажать Load - переместить нужную ВМ в Virtual machines to import - Next - выбрать параметры импорта и нажать OK

<br>
