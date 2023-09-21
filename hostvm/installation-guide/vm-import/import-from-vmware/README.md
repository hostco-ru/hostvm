# Конвертация ВМ из VMware

### Конвертация и импорт OVA с помощью утилиты virt-v2v

Данный способ предназначен для импорта OVA, и является рекомендуемым, т.к. сразу определяется текущая конфигурация и встраиваются необходимые драйверы.

[Список поддерживаемых утилитой гостевых ОС](https://libguestfs.org/virt-v2v-support.1.html)

Пример использования утилиты, для конвертации и загрузки в Export домен HOSTVM:

поместите Virtual Appliance на хост виртуализации и выполните команду

<pre class="language-bash"><code class="lang-bash"><strong>LIBGUESTFS_BACKEND=direct virt-v2v -i ova /path/to/vm.ova -o rhv -of qcow2 -os /path/to/export_domain
</strong></code></pre>

где:

/path/to/vm.ova - полный путь к файлу Virtual Appliance

/path/to/export\_domain - путь к экспорт-домену HOSTVM, в виде server:/path/to/esd

[Полное описание и перечень опций](https://libguestfs.org/virt-v2v.1.html)

### Конвертация и импорт виртуальных машин с помощью сценария импорта

Данный способ предназначен для импорта виртуальных машин, неподдерживаемых утилитой virt-v2v. Требуется [предварительная установка драйверов](../drivers-preinstallation.md) virtio.

Сценарий импорта `import-to-ovirt.pl` доступен для загрузки в [личном кабинете](https://lk.pvhostvm.ru/), расположен в каталоге дистрибутивов в папке `HOSTVM/Misk/VM Convert/`

Пример использования сценария импорта доступен в статье [Конвертация виртуальной машины с ОС AltLinux из VMware в HOSTVM](vmware\_convert\_altlinux.md).
