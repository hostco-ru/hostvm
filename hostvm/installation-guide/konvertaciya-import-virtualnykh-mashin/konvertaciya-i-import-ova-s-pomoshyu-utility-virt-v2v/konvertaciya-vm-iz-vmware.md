# Конвертация ВМ из VMware

Данный способ предназначен для импорта OVA, и является рекомендуемым, т.к. сразу определяется текущая конфигурация и встраиваются необходимые драйверы.

{% hint style="warning" %}
Необходимые драйвера встраиваются при условии предварительно установленного пакета virtio-win на хосте, с которого производится импорт.
{% endhint %}

Установить пакет можно выполнив команду на хосте:

```
dnf install virtio-win
```

​[Список поддерживаемых утилитой гостевых ОС](https://libguestfs.org/virt-v2v-support.1.html)

​Перед выполнением команды загрузите Virtual Appliance на хост виртуализации.

Пример использования утилиты, для конвертации и загрузки в [Export домен](../export-domen.md) HOSTVM:

`LIBGUESTFS_BACKEND=direct virt-v2v -i ova /path/to/vm.ova -o rhv -of qcow2 -os /path/to/export_domain`

где:

/path/to/vm.ova - полный путь к файлу Virtual Appliance\
/path/to/export\_domain - путь к экспорт-домену HOSTVM, в виде server:/path/to/esd

\
​[Полное описание и перечень опций](https://libguestfs.org/virt-v2v.1.html)​
