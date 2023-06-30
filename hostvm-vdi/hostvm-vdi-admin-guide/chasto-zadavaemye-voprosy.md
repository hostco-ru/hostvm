# Часто задаваемые вопросы

### Импорт апплаенса в VMware

Формат апплаенсов HOSTVM VDI не поддерживается со стороны VMware.

Возможна конвертация дисков в формат vmdk с помощью утилиты qemu-img (есть в составе Linux-подобных операционных систем), и импорт их в VMWare.

Распакуйте .ova файл брокера: tar xvf hostvm-vdi.ova На выходе будет 2 файла: vm.ovf и сам диск (например c9bfb5c7-edc0-4596-adca-f63cca42b4c9)

Конвертируйте его командой: qemu-img convert -f qcow2 c9bfb5c7-edc0-4596-adca-f63cca42b4c9 -O vmdk hostvm-vdi.vmdk Поскольку xml с настройками машины не импортируется, их придется выставить вручную в настройках после импорта.

Также для конвертации vmdk имеются следующие ограничения: VMDK disks converted through qemu-img are always monolithic sparse VMDK disks with an IDE adapter type.

