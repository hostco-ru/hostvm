# Конвертация дисков с помощью StarWind V2V Converter

Данный способ конвертации также поддерживается для импорта виртуальных машин в HOSTVM.&#x20;

Потребуется отдельная Windows машина с установленным ПО StarWind V2V Converter.

Порядок действий:

* Скопировать vhdx на Windows машину;
* Запустить ПО Starwind;
* Выбрать local file, указать на конвертируемый жесткий диск;
* Выбрать формат результирующего диска qcow2, далее снова выбрать local file и указать место сохранения результирующего диска;
* Выгрузить получившийся qcow2 диск.

Загрузка в [Export домен](export-domain.md) HOSTVM производится с помощью скрипта import-to-hostvm.pl Загрузить файл сценария конвертации import-to-hostvm.pl на хост HOSTVM, с которого будет выполняться импорт ВМ. Загрузка выполняется из личного кабинета [https://lk.pvhostvm.ru/](https://lk.pvhostvm.ru/) Сценарий import-to-hostvm.pl  расположен в каталоге дистрибутивов в папке HOSTVM/Misc/VM Convert/

```
export LIBGUESTFS_BACKEND=direct
./import-to-hostvm.pl /path/to/disk.qcow2 /path/to/export_domain
```
