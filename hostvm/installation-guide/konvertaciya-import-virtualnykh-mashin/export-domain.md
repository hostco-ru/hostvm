# Export Domain

Export Domain - временное хранилище, которое используется для копирования и перемещения образов между дата-центрами и площадками HOSTVM.&#x20;

Export Domain'ы можно использовать для резервного копирования виртуальных машин.&#x20;

Export Domain можно перемещать между дата-центрами, однако он может быть активен только в одном дата-центре одновременно.&#x20;

Export Domain'ы могут быть созданы только на основе NFS. В дата-центр можно добавить только один Export Domain.

#### Подготовка NFS-share

Для подготовки nfs-share воспользуйтесь соответствующей [инструкцией](../../rukovodstvo-po-administrirovaniyu/khranilishe-dannykh/podgotovka-i-podklyuchenie-nfs-khranilisha/podgotovka-nfs-khranilisha.md).

#### Добавление в дата-центр

Для добавления Export Domain'а воспользуйтесь соответствующей [инструкцией](../../rukovodstvo-po-administrirovaniyu/khranilishe-dannykh/podgotovka-i-podklyuchenie-nfs-khranilisha/dobavlenie-nfs-khranilisha.md). При добавлении в дата-центр в качестве Domain Function выбрать Export.&#x20;

Доступные для импорта виртуальные машины будут доступны на вкладе VM Import соответствующего Export Domain'а:

<figure><img src="../../../.gitbook/assets/image (105).png" alt=""><figcaption></figcaption></figure>

