# Export домен

Export домен - временное хранилище, которое используется для копирования и перемещения образов между дата-центрами и площадками HOSTVM.&#x20;

Export домены можно использовать для резервного копирования виртуальных машин.&#x20;

Export домен можно перемещать между дата-центрами, однако он может быть активен только в одном дата-центре одновременно.&#x20;

Export домены могут быть созданы только на основе NFS. В дата-центр можно добавить только один Export домен.

#### Подготовка NFS-share

Для подготовки nfs-share воспользуйтесь соответствующей [инструкцией](../../rukovodstvo-po-administrirovaniyu/khranilishe-dannykh/podgotovka-i-podklyuchenie-nfs-khranilisha/podgotovka-nfs-khranilisha.md).

#### Добавление в дата-центр

Для добавления Export домена воспользуйтесь соответствующей [инструкцией](../../rukovodstvo-po-administrirovaniyu/khranilishe-dannykh/podgotovka-i-podklyuchenie-nfs-khranilisha/dobavlenie-nfs-khranilisha.md). При добавлении в дата-центр в качестве Domain Function выбрать Export.&#x20;

#### Импорт виртуальных машин из Export домена

`HOSTVM Manager GUI - Compute - Virtual machines` - нажать 3 точки в меню - Import - выбрать нужный дата центр, source=export domain - нажать Load - переместить нужную ВМ в Virtual machines to import - Next - выбрать параметры импорта и нажать OK

Доступные для импорта виртуальные машины отражены на вкладе VM Import соответствующего Export домена, откуда также могут быть импортированы:

<figure><img src="../../../.gitbook/assets/image (105) (1).png" alt=""><figcaption></figcaption></figure>
