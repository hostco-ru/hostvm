# Включение promiscuous mode для сети гипервизора для вложенной виртуализации

В некоторых случаях при использовании вложенной виртуализации могут возникнуть проблемы при подключении ко вложенной виртуальной машине. Для решения можно выключить Network Filter для сети (аналог promiscuous mode в гипервизорах VMware), к которой подключен вложенный гипервизор.

1. Зайдите на административный портал HOSTVM Manager, перейдите во вкладку **Network => Networks**.
2. Кликните на сеть, для которой хотите изменить параметры, перейдите на вкладку **vNIC Profiles** и нажмите кнопку **Edit**.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

3. В выпадающем меню **Network Filter** измените значение на _No Network Filter_.

<figure><img src="../../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
