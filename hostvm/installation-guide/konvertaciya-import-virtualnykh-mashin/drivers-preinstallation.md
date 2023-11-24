# Предварительная установка драйверов

HOSTVM использует тип устройства Virtio для подключения дисков виртуальных машин. В случаях, когда для импорта не используется утилита virt-v2v, в гостевую ОС необходимо  заранее загрузить драйверы для устройств Virtio. Ниже описан перечень действий в гостевых ОС для загрузки этих драйверов.

### Alt Server 8.x, 9.x

Установить гостевые утилиты:

`apt-get install ovirt-guest-agent`

Добавить модули virtio в конфигурационный файл `/etc/initrd.mk`:

`MODULES_ADD += virtio_blk virtio_scsi virtio_pci`

`MODULES_PRELOAD += virtio_blk virtio_scsi virtio_pci`

Сгенерировать новый initramfs:

`make-initrd`

### CentOS 7.x, 8.x

Установить гостевые утилиты:

`yum install ovirt-guest-agent-common`

Добавить модули virtio в initramfs:

`dracut --force --no-hostonly`

Проверить, что модули загружены:

`lsinitrd /boot/initramfs-<kernel.version>.img | grep -i virtio`

`<kernel.version>` - текущая версия ядра, можно посмотреть командой:

`uname -r`
