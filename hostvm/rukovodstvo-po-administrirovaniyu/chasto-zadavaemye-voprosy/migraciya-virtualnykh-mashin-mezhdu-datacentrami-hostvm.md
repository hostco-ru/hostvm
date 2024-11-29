# Миграция виртуальных машин между датацентрами HOSTVM

Датацентры являются отдельными объектами в среде HOSTVM, между этими двумя объектами нет общих путей хранения. Следовательно, для перемещения виртуальных машин между датацентрами необходимо установить внешнее подключение к обоим датацентрам через домен экспорта.

Процесс миграции включает в себя:

1\) Создание домена экспорта и подключение его к датацентру1:

Для создания домена экспорта можно использовать nfs-share, с процедурой ее создания можно ознакомиться в главе [Подготовка NFS share](../../installation-guide/ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-cli/pered-ustanovkoi/podgotovka-nfs-share.md)

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

2\) Перенос виртуальных машин в домен экспорта из datacenter1:

`Compute -> Virtual Machines -> выбирате ВМ -> ⋮ -> Export to Export Domain`

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3\) Отсоединение домена экспорта от датацентра1:

`Data Centers -> выбираете data center -> выбираете Export Domain -> Maintenance -> Detach`

<figure><img src="../../../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

Далее `Storage -> domains -> выбираете export domain -> remove`

<figure><img src="../../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
**Важно!** Чек-бокс Format Domain отмечать не нужно, иначе все данные будут утеряны.
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

4\) Подключение &#x434;_&#x43E;мена экспорта к датацентру2:_

`Storage -> Domains -> Import Domain`

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

После добавления домена активируйте его:

`Data Centers -> выбираете datacenter -> domain -> activate`

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

5\) Импорт виртуальных машин в datacenter2:

`Compute -> Virtual Machines -> ⋮ -> Import -> Load`

В поле Virtual Machines on Source будут отображены доступные ВМ для импорта

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
