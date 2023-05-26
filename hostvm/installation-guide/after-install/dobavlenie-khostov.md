# Добавление хостов

Для того, чтобы подготовить второй и последующие серверы к работе на них необходимо установить ОС HOSTVM Node. Инструкция по установки приведена на странице [Установка HOSTVM Node на диски СХД](https://kb.pvhostvm.ru/hostvm/installation-guide/installation-hostvm). или странице "[Установка HOSTVM Node на локальные диски](https://hostvm.gitbook.io/hostvm/hostvm/installation-guide/installation-hostvm-on-local-disks)"

После установки ОС, подключитесь к серверу с помощью [PuTTY](https://www.putty.org) под пользователем root.&#x20;

Убедитесь, что сервер "видит" диск, на котором размещены виртуальные машины. Командой `multipath -ll` выведете доступные по FC диски. Из примера ниже видим, что диск с guid `3600508b400099f8e0002e000036a0000`, который использовался для установки первой ноды, подключен.

```
[root@testname2 ~]# multipath -ll
3600508b400099f8e0002e000036a0000 dm-3 HP      ,HSV300
size=250G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='service-time 0' prio=50 status=active
| |- 1:0:3:1 sdg 8:96  active ready running
| `- 2:0:0:1 sdb 8:16  active ready running
`-+- policy='service-time 0' prio=10 status=enabled
  |- 1:0:0:1 sda 8:0   active ready running
  `- 2:0:3:1 sdh 8:112 active ready running
```

Имя управляющей виртуальной машины должно разрешаться на сервере. В файл `/etc/hosts` с помощью редактора nano добавьте запись в формате `<ip-адрес управляющей машины> <engine.<domain>.ru` которая сообщает системе адрес и имя управляющей машины.

Установим nano

```
yum install nano -y
```

Откроем файл и внесем в него изменения. В примере ниже мы сообщили системе, что по адресу `10.1.140.15` расположена управляющая машина с именем `engine.testdomain.ru`

```
nano /etc/hosts
#
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.1.140.15     engine.testdomain.ru
```

Клавишей 'ctrl'+'o' сохранение файл

Клавишей 'ctrl'+'x' выйдите из редактора

Командой `ping -c 4 engine.testdomain.ru` убедитесь, что адрес доступен для сервера

```
[root@testname1 ~]# ping engine.testdomain.ru -c 4
PING engine.testdomain.ru (10.1.140.15) 56(84) bytes of data.
64 bytes from engine.testdomain.ru (10.1.140.15): icmp_seq=1 ttl=64 time=0.250 ms
64 bytes from engine.testdomain.ru (10.1.140.15): icmp_seq=2 ttl=64 time=0.210 ms
64 bytes from engine.testdomain.ru (10.1.140.15): icmp_seq=3 ttl=64 time=0.238 ms
64 bytes from engine.testdomain.ru (10.1.140.15): icmp_seq=4 ttl=64 time=0.200 ms

--- engine.testdomain.ru ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.200/0.224/0.250/0.025 ms
```

Откройте панель управления, перейдите в `Compute -> Hosts`, нажмите кнопку `New`

<figure><img src="../../../.gitbook/assets/Screenshot_4 (4).png" alt=""><figcaption></figcaption></figure>

В открывшемся окне заполните поля `Name`, `Hostname`(достаточно указать ip-адрес), `Password` от учетной записи root.&#x20;

<figure><img src="../../../.gitbook/assets/post-install-host-2 (1).jpg" alt=""><figcaption></figcaption></figure>

Чтобы добавить возможность запуска виртуальной машины Hosted engine, при добавлении хоста на вкладке Hosted Engine выберите Deploy из выпадающего списка:

<figure><img src="../../../.gitbook/assets/he-deploy (1).png" alt=""><figcaption></figcaption></figure>

Для обеспечения высокой доступности рекомендуется иметь хотя бы 2 таких хоста. Обычный хост виртуализации не имеет возможности запускать виртуальную машину Hosted engine.

После завершения установки, оба хоста будут доступны для работы:

<figure><img src="../../../.gitbook/assets/post-install-host-3 (1).jpg" alt=""><figcaption></figcaption></figure>

Если же на этом этапе возникла ошибка, то необходимо зайти в параметры хоста, нажав на его имя, перейти в вкладку Events и посмотреть, на каком этапе прервалась установка ПО виртуализации на хост.

<figure><img src="../../../.gitbook/assets/image (4) (2).png" alt=""><figcaption></figcaption></figure>

Также, более подробно причину ошибки можно рассмотреть в логах хоста по пути /var/log/ovirt-hosted-engine-setup с актуальным временем создания.

Обычно причиной проблем являются:

1\) Недоступность интернета на хосте, или невозможность отрезолвить DNS имя репозитория (нужно проверить сетевые настройки хоста)

2\) Проблемы с скачиванием пакетов из репозитория (можно его поменять с repo.hostco.ru на repoext.hostco.ru или наоборот)

3\) Проблемы с зависимостями пакетов при установке - нужно сообщить о этой проблеме в ГК ХОСТ, для исправления зависимостей в репозитории.
