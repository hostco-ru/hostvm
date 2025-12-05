# Заполнение формы для установки значений переменных

Перед началом работы рекомендуется заполнить последний столбец следующей таблицы (способ сбора данных для таблице описан ниже по тексту):

| Название                               | Как узнать | Значение    |
| -------------------------------------- | ---------- | ----------- |
| ip для engine                          | -          | <p><br></p> |
| ip сервера                             | ip addr    | <p><br></p> |
| ip шлюза по умолчанию                  | ip route   | <p><br></p> |
| ip dns-сервера                         | -          | <p><br></p> |
| домен установки                        | -          | <p><br></p> |
| hostname сервера                       | -          | <p><br></p> |
| название интерфейса                    | ip addr    | <p><br></p> |
| предпочтительный gluster-hostname      | -          | <p><br></p> |
| предпочтительное название тома gluster | -          | <p><br></p> |
| директорию для размещения тома gluster | -          | <p><br></p> |

Для получения ip-адреса сервера и название интерфейса выполните команду `ip addr` :

Согласно примеру ниже видно, что ip-адрес сервера - `10.1.158.140`, название интерфейса - `eno1`.

```
[root@virt2 ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defaul t qlen 1000
link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
inet 127.0.0.1/8 scope host lo
valid_lft forever preferred_lft forever
inet6 ::1/128 scope host
valid_lft forever preferred_lft forever
2: enp8s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
link/ether 5c:f3:fc:11:42:90 brd ff:ff:ff:ff:ff:ff
3: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group defa ult qlen 1000
link/ether 5c:f3:fc:11:42:91 brd ff:ff:ff:ff:ff:ff
inet 10.1.158.140/24 brd 10.1.158.255 scope global noprefixroute eno1
valid_lft forever preferred_lft forever
inet6 fe80::e54:1202:33cf:5a8a/64 scope link noprefixroute
valid_lft forever preferred_lft forever
4: enp0s29u1u1u5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast st ate UNKNOWN group default qlen 1000
link/ether 5e:f3:fc:15:16:bb brd ff:ff:ff:ff:ff:ff
inet 169.254.95.120/24 brd 169.254.95.255 scope link noprefixroute dynamic e np0s29u1u1u5
valid_lft 346sec preferred_lft 346sec
inet6 fe80::42ed:eae9:47ac:6b89/64 scope link noprefixroute
valid_lft forever preferred_lft forever
```

Для получения ip-адреса шлюза выполните команду `ip route`.

Согласно примеру ниже видно, что ip шлюза по умолчанию - `10.1.158.1`

```
[root@virt2 ~]# ip route
default via 10.1.158.1 dev eno1 proto static metric 100
10.1.158.0/24 dev eno1 proto kernel scope link src 10.1.158.140 metric 100
169.254.95.0/24 dev enp0s29u1u1u5 proto kernel scope link src 169.254.95.120 metric 101
```

Для параметров, связанных с glusterfs, могут быть выбраны значения по умолчанию:

| Название                               | Значение по умолчанию        |
| -------------------------------------- | ---------------------------- |
| предпочтительный gluster-hostname      | glusternode1                 |
| предпочтительное название тома gluster | hosted-engine                |
| директорию для размещения тома gluster | /data/gluster/hosted\_engine |

Обратите внимание, что рекомендуется использовать отдельный раздел для размещения тома glusterfs, который имеет точку монтирования в директории /data<br>
