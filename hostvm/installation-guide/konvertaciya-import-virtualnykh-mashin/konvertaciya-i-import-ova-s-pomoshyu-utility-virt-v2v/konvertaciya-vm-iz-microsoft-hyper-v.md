# Конвертация ВМ из Microsoft Hyper-V

Данный способ подходит для машин с одним диском, и является рекомендуемым, т.к. сразу определяется текущая конфигурация и встраиваются необходимые драйверы.\
[Список поддерживаемых утилитой гостевых ОС](https://libguestfs.org/virt-v2v-support.1.html)​

Перед выполнением команды загрузите образ диска vhdx на хост виртуализации.\
Пример использования утилиты, для конвертации и загрузки в Export домен HOSTVM:

`virt-v2v -i disk /path/to/disk.vhdx -o rhv -of qcow2 -os /path/to/export_domain`

где:

/path/to/disk.vhdx - полный путь к образу диска vhdx\
/path/to/export\_domain - путь к экспорт-домену HOSTVM, в виде server:/path/to/esd

[Полное описание и перечень опций](https://libguestfs.org/virt-v2v.1.html)

\
