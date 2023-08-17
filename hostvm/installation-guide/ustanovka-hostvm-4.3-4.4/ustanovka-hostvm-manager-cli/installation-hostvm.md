# Установка HOSTVM Node на диски СХД (glusterfs)

## Перед установкой

Перед установкой подготовьте на вашей схд системный лун и лун для хранения виртуальных машин. Выполните маппинг сервера к выделенным ему лунам(vdisk'ам) по инструкциям от производителя вашего серверного оборудования. В случае использования системного луна, подключенного по FC, настройте bootFromSan по инструкциям от производителя вашего серверного оборудования.

Для установки необходимо использовать iso-образ HOSTVM Node, который доступна в [наборе дистрибутивов для развертывания решения](https://lk.pvhostvm.ru/Download)

Установка как гипервизора, так и менеджера виртуализации производится на физический сетевой интерфейс. Порты коммутатора на время установки должны быть в режиме access. Конфигурирование VLAN’ов, объединение сетевых интерфейсов и т.д. производится после установки менеджера виртуализации в его веб-интерфейсе.

Подключите полученный iso-образ к серверу, запустите сервер.

## Видео-инструкция по установке

{% embed url="https://cloud.hostco.ru/s/YRW2PNdSoZQdTii" %}

01:07 - Перезагрузка и монтирование образа операционной системы

04:09 - Инсталляция образа

54:00 - Установка пакетов виртуализации

01:39:06 - Добавление записи в файл hosts

01:40:31 - Авторизация в HOSTVM Engine

## Процесс установки

При загрузке откроется меню выбора действия. За 60 секунд выберите _Install HOSTVM Node_ Если за 60 секунд после загрузки не выбрать данный пункт, то начинается тестирование ресурсов сервера и только после этого начнется установка. Остановить тестирование ресурсов сервера возможно через нажатие клавиши _esc_.

![](<../../../../.gitbook/assets/Screenshot\_2 (6).png>)

В случае если загрузка установщика зависнет, то нужно повторно загрузиться с установочного диска и в стартовом меню действий выбрать пункт «Troubleshooting», затем “Install HOSTVM Node 4.3.9 in basic graphics mode” для запуска установки с использованием псевдографического интерфейса.

В открывшемся окне выберите английский язык (English), который будет использоваться в интерфейсе установщика.

_Выбранный язык не влияет на язык внутри самой операционной системы, которая устанавливается без графической оболочки._

Скриншоты инструкции выполнены в интерфейсе с английским языком. Нажмите _Continue_.

![](../../../../.gitbook/assets/Screenshot\_3.png)

Далее автоматически открывается меню настроек.

![](../../../../.gitbook/assets/Screenshot\_4.png)

Перейдите в _DATE & TIME_, укажите ваш часовой пояс, время и дату. Нажмите _Done_.

![](../../../../.gitbook/assets/Screenshot\_5.png)

**ВАЖНО!: Для корректной установки, необходимо, чтобы серверу, на этапе установки, был задан корректный IPv4 адрес, который имеет доступ в интернет (для доступа к репозиториям). Также должен быть указан корректный DNS сервер, и необходимо задать корректное имя хоста, отличное от localhost.localdomain, во избежание возможных ошибок во время установки HOSTVM Manager.**

Перейдите в _NETWORK & HOST NAME_.

![](../../../../.gitbook/assets/Screenshot\_1.png)

Выберите интерфейс, нажмите кнопку _Configure..._. В открывшемся окне перейдите на вкладку _IPv4 Settings_, выберите _Method: Manual_, введите ip, маску, gw, DNS-сервер. Нажмите кнопку _Save_.

![](<../../../../.gitbook/assets/Screenshot\_2 (1).png>)

Переведите тригер возле названия подключения в положение _On_. В поле _Host name_ введите имя сервера, нажмите кнопку _Apply_. Нажмите кнопку _Done_.

![](<../../../../.gitbook/assets/Screenshot\_5 (1).png>)

Перейдите в _Instalation Destination_. Выберете диск на который необходимо выполнить установку. Выберете радиокнопку _I will configure partitioning_. Нажмите _Done_.

![](<../../../../.gitbook/assets/Screenshot\_6 (1).png>)

Если диск не отображается, необходимо открыть дополнительное окно под кнопкой _Add a disk..._, поставить галочку напротив необходимого диска и нажать кнопку _Done_.

![](<../../../../.gitbook/assets/Screenshot\_8 (1).png>)

После выбора места установки автоматически открывается следующее меню.

![](../../../../.gitbook/assets/centos7-install-9-1.jpg)

Если диск уже использовался (имел таблицу разделов), то удалите их, как показано ниже.

![](../../../../.gitbook/assets/centos7-install-9-2.jpg)

![](../../../../.gitbook/assets/centos7-install-9-3.jpg)

Когда на диске не останется существующих разделов Из выпадающего меню выберите _LVM Thin Provisioning._ Нажмите _Click here to create them automatically_.

![](<../../../../.gitbook/assets/Screenshot\_1 (1) (1).png>)

Удалите _home_, как показано ниже.

![](<../../../../.gitbook/assets/Screenshot\_2 (2).png>)

Освободившееся место отдайте разделу _/_. Для этого выберете его, укажите в поле Desired Capacity его размер. Минимальный размер диска: 45GB. Измените фокус (выберете другой раздел), чтобы изменения отобразились на экране.

![](<../../../../.gitbook/assets/Screenshot\_3 (1).png>)

Добавьте раздел /var. Присвойте ему не менее 15GB свободного места

![](<../../../../.gitbook/assets/Screenshot\_4 (2).png>)

Нажмите _Done_.

![](<../../../../.gitbook/assets/Screenshot\_6 (2).png>)

Подтвердите действие кнопкой _Accept Changes_.

![](../../../../.gitbook/assets/Screenshot\_7.png)

В стартовом меню нажмите кнопку _Begin Instalation_, чтобы начать установку.

![](<../../../../.gitbook/assets/Screenshot\_9 (1).png>)

В открывшемся окне выберете _Root Password_. Введите ваш пароль (рекомендуемый пароль **engine**). Дважды нажмите _Done_.

![](../../../../.gitbook/assets/Screenshot\_27.png)

![](../../../../.gitbook/assets/Screenshot\_28.png)

Пароль root по умолчанию для hostvm node версии 4.4: HostvmNode.

Если на этом этапе возникнет ошибка с postinstall скриптом, то это означает, что:

![](<../../../../.gitbook/assets/image (29).png>)

1\) Сервер не имеет доступа в интернет и не смог скачать установочные скрипты из репозитория

2\) Имя репозитория не распознано, т.к. DNS сервер не задан или не имеет доступа в интернет

3\) ISO образ для установки скачан давно, и некоторые пути в скриптах установки изменились - нужно скачать ISO заново и выполнить установку с него.

Альтернативно, можно выполнить данный скрипт вручную из ОС (смотри раздел - [прокси и репозиторий](https://kb.pvhostvm.ru/hostvm/installation-guide/installation-hostvm#nastroika-proksi-esli-ispolzuetsya-i-repozitoriya)).

Ожидайте окончания установки. После завершения подтвердите перезагрузку нажатием на кнопку _Reboot_.

![](../../../../.gitbook/assets/Screenshot\_30.png)

## Подготовка сервера к развертыванию на диски схд

### Подготовка putty к работе

Убедитесь, что требования, описанные на странице [Системные требования](../../requirements.md) выполняются.

С помощью программы [PuTTY](https://www.putty.org), которая доступна в [наборе дистрибутивов для развертывания решения](https://lk.pvhostvm.ru/Download), под пользователем root подключитесь к серверу.

Перед началом работы рекомендуется настроить логирование сессии putty. Для этого нужно выполнить следующие действия

1. Сохраните имя сервера:

![](../../../../.gitbook/assets/hostvm-install-2.jpg)

1. Перейдите на вкладку Журнал (мы можем добавить ссылку на страницу с этой вкладкой?), выберите `Весь вывод`, укажите путь до файла логов в следующем виде: `C:\path\to\log\hostname-&H-&Y&M&D-&T.log`. Часть `&H-&Y&M&D-&T` указывает, что файл с логом будет создаваться для каждой сессии и автоматически указывать время и дату ее начала:

![](../../../../.gitbook/assets/hostvm-install-3.jpg)

1. Перейдите на вкладку Сеанс, нажмите кнопку `Сохранить`, нажмите клавишу `Enter` чтобы запустить сессию:

![](../../../../.gitbook/assets/hostvm-install-4.jpg)

### Проверить, что диск предназначенный для размещения виртуальных машин подключен

Командой `multipath -ll` выведете подключенные по FC устройства. Если диск нужного размера отсутствует, проверьте, что маппинг настроен верно, что на схд диск презентован серверу, что настройки диска и сервера на стороне схд выполнены верно. После этого выполните процедуру переобнаружения дисков:

1.  Узнайте количество host bus адаптеров, которые есть на сервере:

    ```
    # ls /sys/class/fc_host
    host0  host1
    ```
2.  Запустите сканирование:

    ```
    echo "1" > /sys/class/fc_host/host0/issue_lip
    echo "- - -" > /sys/class/scsi_host/host0/scan
    echo "1" > /sys/class/fc_host/host1/issue_lip
    echo "- - -" > /sys/class/scsi_host/host1/scan
    ```

    `host0` и `host2` замените на значения, полученные в предыдущем шаге
3.  Перезапустите службу multipathd

    ```
    service multipathd restart
    ```
4.  Проверьте, что необходимый диск стал доступен

    ```
    multipath -ll
    ```

### Настройка прокси (если используется) и репозитория

{% hint style="warning" %}
Примечание: данный способ настройки прокси может быть применён только для HOSTVM версии 4.3, для HOSTVM 4.4 при наличии прокси необходимо использовать образ с поддержкой установки в offline-режиме.
{% endhint %}

Если в данной сети доступ во вне доступен только через прокси, то следует выполнить следующие настройки:

1. В файле /etc/yum.conf, для загрузки пакетов через прокси добавляем строки

```
proxy=http://proxyhost:8080
proxy_username=proxyname
proxy_password=proxypass
```

1. Экспортировать глобальные переменные прокси для работы CURL

```
## http прокси с именем и паролем 
export http_proxy=http://user:password@your-proxy-ip-address:port/

## HTTPS версия ##
export https_proxy=https://your-ip-address:port/
export https_proxy=https://user:password@your-proxy-ip-address:port/
```

На примере, сначала устанавливаем переменные для прокси и затем используем CURL

```
export http_proxy=http://foo:bar@1.1.1.1:3128/
export https_proxy=$http_proxy
## Use curl command ##
curl -I www.system-admins.ru
```

Если на этапе установки возникала проблема с postinstall скриптом, то необходимо выполнить скрипт `initial.sh`

```
[root@virt2 ~]# sh initial.sh
```

По итогу выполнения скрипта, в директории /root/ будет создан лог-файл `initial_log_текущая-дата.log`

## Заполнение формы для установки значений переменных

### Сбор данных для заполнения формы

Перед началом работы рекомендуется заполнить последний столбец следующей таблицы (_способ сбора данных для таблице описан ниже по тексту)_:

| Название                            |   Как узнать  | Значение |
| ----------------------------------- | :-----------: | :------: |
| ip для engine                       |       -       |          |
| ip сервера                          |    ip addr    |          |
| ip шлюза по умолчанию               |    ip route   |          |
| ip dns-сервера                      |       -       |          |
| домен установки                     |       -       |          |
| hostname сервера                    |       -       |          |
| название интерфейса                 |    ip addr    |          |
| guid на который выполняем установку | multipath -ll |          |

Для получения ip-адреса сервера и название интерфейса выполните команду `ip addr` :

Согласно примеру ниже видно, что ip-адрес сервера - `10.1.140.14`, название интерфейса - `enp3s0`.

```
[root@host1 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:17:a4:77:00:0c brd ff:ff:ff:ff:ff:ff
    inet 10.1.140.14/25 brd 10.1.140.127 scope global noprefixroute enp3s0
       valid_lft forever preferred_lft forever
    inet6 fe80::b50a:7c22:2229:b169/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp7s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:17:a4:77:00:0e brd ff:ff:ff:ff:ff:ff
```

Для получения ip-адреса шлюза выполните команду `ip route`.

Согласно примеру ниже видно, что ip шлюза по умолчанию - `10.1.140.1`

```
[root@host1 ~]# ip route
default via 10.1.140.1 dev enp3s0 proto static metric 100
10.1.140.0/25 dev enp3s0 proto kernel scope link src 10.1.140.14 metric 100
```

Для получения guid диска выполните команду `multipath -ll`

### Запуск программы-помощника IP-wizard

Запустите `IP-wizard.sh`, чтобы подготовить файлы переменных к работе. Следуйте указаниями инструкции в программе:

```
sh IP-wizard.sh 

Добро пожаловать в программу-помощник IP-wizard Группы компаний ХОСТ!
Мы попросим ответить на несколько вопросов и сформируем нужные файлы конфигурации Ansible.

Внимание! Программа изменит файлы в папках /etc/ansible/group_vars и /etc/ansible/host_vars!

Нажмите ENTER для продолжения или ^C для выхода из программы!
Для принятия значений по-умолчанию просто нажимайте ENTER. ;)

Укажите ваш Домен: hostco.ru
Домен: hostco.ru

Укажите DNS имя для oVirt Engine(консоли,веб-интерфейса управления)(без домена)[по умолчанию: engine]: engine-test
DNS имя консоли(веб-интерфейса) управления: engine-test

Укажите  IP адрес для oVirt Engine(консоли,веб-интерфейса управления): 10.1.140.15
IP адрес HostedEngine: 10.1.140.15

На данном сервере имеются интерфейсы:
lo : 127.0.0.1
enp2s0f0 : 10.1.140.13

Укажите имя интерфейса первого сервера, которое будет использовано как виртуальный коммутатор(например enp2s0f0). Список интерфейсов выведен выше. : enp2s0f0
имя интерфейса первого сервера: enp2s0f0

Укажите доступный клиентам IP адрес этого сервера [по умолчанию: 10.1.140.13] :
IP адрес текущего сервера: 10.1.140.13

Укажите шлюз (gateway) сети этого сервера[по умолчанию: 10.1.140.1] :
Шлюз текущего сервера: 10.1.140.1

Укажите hostname текущего хоста (без домена)[по умолчанию ovirt3]:
hostname текущего сервера: ovirt3


Укажите ваш DNS сервер[по умолчанию: 10.1.64.248]:
DNS: 10.1.64.248
Укажите тип хранилища для размещения oVirt Engine(консоли,веб-интерфейса управления)(fc или glusterfs)[по умолчанию: glusterfs]: glusterfs
Тип хранилища HostedEngine: glusterfs

На сервере будет развернута нода glusterfs.
Укажите предпочтительный gluster-hostname первого сервера (без домена)(glusternode1): glust1
gluster-hostname первого сервера: glust1

В среде виртуализации для размещения управляющей машины engine будет создан домен хранения hosted-engine
Укажите предпочтительное название тома gluster (hosted-engine): 
Имя тома gluster для домена хранения hosted-engine: hosted-engine

Укажите директорию для размещения тома gluster hosted-engineОбратите внимание, что для установки на разделе выбранного расположения директории должно быть свободно минимум 61ГБ
(/data/gluster/hosted_engine): 
Директория для glusterfs : /data/gluster/hosted_engine

Начинаю модификацию файлов...
dns_root: 10.1.64.248
Изменен файл /etc/ansible/group_vars/all.

Генерируем /etc/ansible/group_vars/nodes...
ansible_connection: ssh 
ansible_ssh_user: root 
ansible_ssh_pass: engine
ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
ovirt_engine_ip: 10.1.140.15
ovirt_engine_fqdn: 'engine-test.mydomain.ru'
ovirt_engine_domain: 'mydomain.ru'
ovirt_engine_password: 'engine'
Изменен файл /etc/ansible/group_vars/nodes.

Генерируем /etc/ansible/host_vars/host1...
hostname: myhost1.mydomain.ru
shothostname: myhost1
ip: 10.1.140.13
ip_gateway: 10.1.140.1
nic_for_ovirtmgmt_bridge: enp2s0f0
gluster_hostname: glust1
Изменен файл /etc/ansible/host_vars/host1.

Генерируем /etc/ansible/group_vars/gluster...
gluster_dir_for_hosted_engine: /data/gluster/hosted_engine/brick1
gluster_hosted_engine_volume_name: hosted-engine
Изменен файл /etc/ansible/group_vars/gluster.

Правим файл ответов для hosted-engine...
Файл ответов обновлен.

[root@host1 ~]#
```

## Установка виртуализации

Выполните команду `ansible-playbook /etc/ansible/make-prepare.yml`, чтобы подготовить к работе /etc/hosts.

Выполните команду `ansible-playbook /etc/ansible/make-gluster-storages.yml`, чтобы подготовить к работе glusterfs.

```
[root@host1 ~]# ansible-playbook /etc/ansible/make-gluster-storages.yml
```

Запустите установку необходимых пакетов виртуализации командой `ansible-playbook /etc/ansible/make-ovirt.yml`. На ее выполнение уйдет чуть больше часа.

```
[root@host1 ~]# ansible-playbook /etc/ansible/make-ovirt.yml
[DEPRECATION WARNING]: The TRANSFORM_INVALID_GROUP_CHARS settings is set to allow bad characters in group names by default, this will change, but still be user configurable on deprecation. This feature will be removed in version 2.10.
Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
 [WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details


PLAY [ovirt-master] *************************************************************************************************************************************************************************************************************************

TASK [Gathering Facts] **********************************************************************************************************************************************************************************************************************
ok: [localhost]

TASK [ovirt-master : Ensure that ovirt-release43.rpm is installed] **************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple items and specifying `name: "{{ item }}"`, please use `name:
['http://resources.ovirt.org/pub/yum-repo/ovirt-release4
ok: [localhost] => (item=[u'http://resources.ovirt.org/pub/yum-repo/ovirt-release43.rpm'])
3.rpm']` and remove the loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
TASK [ovirt-master : Ensure that ovirt-hosted-engine-setup is installed. !A very long time. Wait!] ******************************************************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple items and specifying `name: "{{ item }}"`, please use `name: ['ovirt-hosted-engine-
setup-2.3.12-1.el7.noarch']` and remove the loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost] => (item=[u'ovirt-hosted-engine-setup-2.3.12-1.el7.noarch'])

TASK [ovirt-master : Ensure that ovirt-engine-appliance is installed. !A very long time. Wait!] *********************************************************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple items and specifying `name: "{{ item }}"`, please use `name: ['ovirt-engine-
appliance-4.3-20190926.1.el7.x86_64']` and remove the loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost] => (item=[u'ovirt-engine-appliance-4.3-20190926.1.el7.x86_64'])

TASK [ovirt-master : Ensure that expect is installed] ***************************************************************************************************************************************************************************************
[DEPRECATION WARNING]: Invoking "yum" only once while using a loop via squash_actions is deprecated. Instead of using a loop to supply multiple items and specifying `name: "{{ item }}"`, please use `name: ['expect']` and remove the
loop. This feature will be removed in version 2.11. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost] => (item=[u'expect'])

TASK [ovirt-master : Ensure that script-hosted-engine-deploy is pushed] *********************************************************************************************************************************************************************
changed: [localhost]

TASK [ovirt-master : Check hosted-deploy status] ********************************************************************************************************************************************************************************************
ok: [localhost]

TASK [ovirt-master : debug] *****************************************************************************************************************************************************************************************************************
ok: [localhost] => {
    "msg": "You must run deploy first"
}

PLAY RECAP **********************************************************************************************************************************************************************************************************************************
localhost                  : ok=9    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

[root@host1 ~]#
```

Сформированный файл `/root/script-hosted-engine-deploy` содержит инструкции, необходимые для развертывания виртуализации Запустите его на исполнение командой `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log`:

```
/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log
```

После завершения развертывания виртуализации откройте браузер и перейдите по адресу [https://engine.mydomain.ru](https://engine.mydomain.ru), чтобы попасть в панель управления.

**Примечание:** для HOSTVM Manager версии 4.3 необходимо дополнительно доустановить набор брендирования и локализации HOSTVM.

1. Загрузите архив HOSTVM-localocalization-branding.zip из каталога загрузок HOSTVM.
2. Подключитесь к виртуальной машине Engine и загрузите на неё архив в любое удобное расположение.
3. Распакуйте архив, перейдите в папку hostvm-localocalization-branding/branding/, и запустите файл makeHostvmBranding.sh:\
   cd hostvm-localocalization-branding/branding/:

```
cd hostvm-localocalization-branding/branding/
sh makeHostvmBranding.sh
```

## Если что-то пошло не так

1. Проверить корректность данных, которые были введены в IP-wizard. При обнаружении ошибки выполните команду `ansible-playbook /etc/ansible/clean-node.yml` и начните сначала.
2. Если на этапе `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` появилась ошибка, то выполните команду `ansible-playbook /etc/ansible/clean-node.yml` и начните сначала
3. Если на этапе `ansible-playbook /etc/ansible/make-prepare.yml` появилась ошибка, повторите выполнение данной команды
4. Если на этапе `ansible-playbook /etc/ansible/make-gluster-storages.yml` появилась ошибка, повторите выполнение данной команды
5. Если на этапе `ansible-playbook /etc/ansible/make-ovirt.yml` появилась ошибка, повторите выполнение данной команды
6. Если после завершения установки вам не открывается страница в браузере с адресом [https://engine.mydomain.ru](https://engine.mydomain.ru), то
   1. Проверьте, что ip для engine, указанный в таблице в начале установки отвечает на команду ping
   2. Проверьте, что имя `engine.mydomain.ru` разрешается вашим dns-сервером.
7. Если на этапе установки engine `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` установка зависает на этапе `Engine VM domain: [rtc.local]rtc.local Enter root password that will be used for the engine appliance: engine`, то подключитесь к консоли сервера не по SSH, а с помощью ipmi(iLO, iDRAC, etc.) и повторно запустите скрипт установки engine.

Схема установки hostvm и самостоятельного решения проблем представлена на рисунке ниже:

![](../../../../.gitbook/assets/troubleshooting-scheme.jpg)

Если устранить проблему не удалось, обратитесь в [техническую поддержку](https://lk.pvhostvm.ru/) используя [инструкцию](https://lk.pvhostvm.ru/) К обращению приложите лог вывода вашей консоли, который был настроен в начале установки и файл `/root/script-hosted-engine-deploy.log`.
