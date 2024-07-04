# Базовые условия для развертывания платформы виртуализации HOSTVM

Если планируется использовать локальные диски серверов (гиперконвергенция/SDS) рекомендуется:

* Минимум три физические машины для развертывания хостов, для создания отказоустойчивой реплики;
* Как минимум два сетевых интерфейса на каждом из хостов для разделения трафика.

Для успешной установки HOSTVM Manager требуется выполнение следующих условий:

* Должны быть настроены FQDN (fully qualified domain name) для HOSTVM Manager и для HOSTVM Node (гипервизор), созданы A-записи на DNS сервере. Если есть затруднения с использованием DNS-сервера, можно создавать записи на хостах и ВМ HOSTVM Manager следуя [https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns](deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns.md)

{% hint style="danger" %}
**Установка HOSTVM Manager должна проводиться на сетевой интерфейс без bonding и VLAN (можно будет добавить после развертывания);**
{% endhint %}

Примерный порядок действий со ссылками на соответствующие инструкции:

1. Установить гипервизор (HOSTVM Node) на каждый задействованный сервер в соответствии с рекомендуемыми системными требованиями [https://kb.pvhostvm.ru/hostvm/installation-guide/requirements](requirements.md)
2. На один из хостов (сервер с платформой виртуализации) установить HOSTVM Manager (ВМ менеджера виртуализации) используя требуемый тип хранилища:

* Вариант установки на NFS -[ https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-nfs](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-4.4.8-gui/ustanovka-hostvm-manager-na-nfs.md)
* Вариант установки с использованием SDS - [https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-gluster](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-gui/ustanovka-hostvm-manager-na-gluster.md)
* Вариант установки на FC-диск - [https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-fc-disk](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-4.4.8-gui/ustanovka-hostvm-manager-na-fc-disk.md)

3\. Далее необходимо выполнить добавление хостов со стороны HOSTVM Manager по инструкции (см. раздел Добавление хостов): [https://kb.pvhostvm.ru/hostvm/installation-guide/after-install#dobavlenie-khostov](https://kb.pvhostvm.ru/hostvm/installation-guide/after-install#dobavlenie-khostov)
