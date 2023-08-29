# Часто задаваемые вопросы

### Импорт апплаенса в VMware

Формат апплаенсов HOSTVM VDI не поддерживается со стороны VMware.

Возможна конвертация дисков в формат vmdk с помощью утилиты qemu-img (есть в составе Linux-подобных операционных систем), и импорт их в VMWare. Для этого распакуйте .ova файл брокера:&#x20;

```
tar xvf hostvm-vdi.ova
```

На выходе будет 2 файла: vm.ovf и диск ВМ (например c9bfb5c7-edc0-4596-adca-f63cca42b4c9)

Конвертируйте диск командой:&#x20;

```
qemu-img convert -f qcow2 c9bfb5c7-edc0-4596-adca-f63cca42b4c9 -O vmdk hostvm-vdi.vmdk
```

Поскольку xml-файл с настройками машины не импортируется, необходимо выставить их вручную в настройках ВМ после импорта.

{% hint style="warning" %}
**Для конвертации vmdk имеются следующие ограничения:**&#x20;

VMDK disks converted through qemu-img are always monolithic sparse VMDK disks with an IDE adapter type.
{% endhint %}
