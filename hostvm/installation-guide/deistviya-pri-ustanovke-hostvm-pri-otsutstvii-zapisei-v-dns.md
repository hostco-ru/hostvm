# Действия при установке HOSTVM при отсутствии записей в DNS

В случае, если на DNS-сервере отсутствует запись о ноде HOSTVM, то нужно вручную внести изменения в файл /etc/hosts, для этого:

Перейдите на вкладку Terminal

![](<../../.gitbook/assets/image (38).png>)

Откройте файл /etc/hosts:

`vi /etc/hosts`

\
и добавьте в конец файла записи вида:

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

10.1.99.61 - IP-адрес ноды

gen9hostvm448 - FQDN ноды

gen9hostvm448.hostco.ru - FQDN.domain.ru ноды

10.1.99.62 - IP-адрес Engine

gen9-engine - FQDN Engine

gen9-engine.hostco.ru - FQDN.domain.ru Engine
