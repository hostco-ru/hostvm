# Импорт апплаенса в VMware

Формат апплаенсов HOSTVM VDI не поддерживается со стороны VMware.

Возможна конвертация дисков в формат vmdk с помощью утилиты qemu-img (есть в составе Linux-подобных операционных систем), и импорт их в VMWare. Для этого распакуйте .ova файл брокера:

```
tar xvf hostvm-vdi.ova
```

На выходе будет 2 файла: vm.ovf и диск ВМ (например c9bfb5c7-edc0-4596-adca-f63cca42b4c9)

Конвертируйте диск командой:

```
qemu-img convert -f qcow2 c9bfb5c7-edc0-4596-adca-f63cca42b4c9 -O vmdk hostvm-vdi.vmdk
```

После выполнения команды вы получите файл vmdk для VMware Workstation. Получившийся vmdk необходимо еще раз конвертировать на хосте esxi:

```
vmkfstools -i source.vmdk -d zeroedthick converted_source.vmdk  
```

Поскольку xml-файл с настройками машины не импортируется, необходимо выставить их вручную в настройках ВМ после импорта.

{% hint style="warning" %}
**Для конвертации vmdk имеются следующие ограничения:**

VMDK disks converted through qemu-img are always monolithic sparse VMDK disks with an IDE adapter type.
{% endhint %}
