# Базовые условия для развертывания платформы виртуализации HOSTVM

Для развертывания платформы виртуализации HOSTVM требуется минимум:

* два физических сервера при использовании внешнего хранилища;
* три физических сервера при использовании локальных дисков серверов (гиперконвергенция/SDS) и минимум два сетевых интерфейса на каждом из серверов для разделения трафика.

Для успешной установки HOSTVM Manager требуется выполнение следующих условий:

* Должны быть настроены FQDN (fully qualified domain name) для HOSTVM Manager и для HOSTVM Node (гипервизор), созданы A-записи на DNS сервере. Если есть затруднения с использованием DNS-сервера, можно создавать записи на хостах и ВМ HOSTVM Manager следуя [https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns](deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns.md)

{% hint style="danger" %}
**Установка HOSTVM Manager должна проводиться на сетевой интерфейс без bonding и VLAN (можно будет добавить после развертывания)**
{% endhint %}

Примерный порядок действий со ссылками на соответствующие инструкции:

1. Установить гипервизор (HOSTVM Node) на каждый задействованный сервер в соответствии с рекомендуемыми системными требованиями [https://kb.pvhostvm.ru/hostvm/installation-guide/requirements](requirements.md)
2. На один из хостов (сервер с платформой виртуализации) установить HOSTVM Manager (ВМ менеджера виртуализации) используя требуемый тип хранилища:

* Вариант установки на NFS -[ https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-nfs](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-4.4.8-gui/ustanovka-hostvm-manager-na-nfs.md)
* Вариант установки с использованием SDS - [https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-gluster](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-4.4.8-gui/ustanovka-hostvm-manager-na-gluster.md)
* Вариант установки на FC-диск - [https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-fc-disk](ustanovka-hostvm-4.3-4.4/ustanovka-hostvm-manager-4.4.8-gui/ustanovka-hostvm-manager-na-fc-disk.md)

3\. Далее необходимо выполнить добавление хостов со стороны HOSTVM Manager по инструкции (см. раздел Добавление хостов): [https://kb.pvhostvm.ru/hostvm/installation-guide/after-install#dobavlenie-khostov](https://kb.pvhostvm.ru/hostvm/installation-guide/after-install#dobavlenie-khostov)

4.При использовании тонких дисков, динамически использующих большое дисковое пространство, рекомендуется настройка системы мониторинга свободного места в доменах хранения. Ситуация, когда хранилище заполнено полностью, является не желательной и может привести к полной потере данных с этого хранилища.&#x20;

Интеграция с Zabbix:  [Конфигурация подключения Zabbix](konfiguraciya-podklyucheniya-zabbix.md)

При отсутствии специализированной системы мониторинга можно воспользоваться штатным средством ovirt-engine-notifier для организации оповещений по протоколу SMTP или SNMP: [Настройка уведомлений](../rukovodstvo-po-administrirovaniyu/nastroika-uvedomlenii/)
