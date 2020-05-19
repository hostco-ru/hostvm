# Установка виртуализации на локальные диски

## Подготовка сервера к развертыванию на локальных дисках

### Подготовка putty к работе

Убедитесь, что требования, описанные на странице [Системные требования](requirements.md) выполняются.

С помощью программы [PuTTY](https://www.putty.org), которая доступна в [наборе дистрибьютивов для развертывания решения](https://reestr.hostco.ru/downloads), под пользователем root подключитесь к серверу.

Перед началом работы рекомендуется настроить логирование сессии putty. Для этого нужно выполнить следующие действия

1. Сохраните имя сервера:

![](../.gitbook/assets/hostvm-install-2.jpg)

2. Перейдите на вкладку Журнал \(мы можем добавить ссылку на страницу с этой вкладкой?\), выберите `Весь вывод`, укажите путь до файла логов в следующем виде: `C:\path\to\log\hostname-&H-&Y&M&D-&T.log`. Часть `&H-&Y&M&D-&T` указывает, что файл с логом будет создаваться для каждой сессии и автоматически указывать время и дату ее начала:

![](../.gitbook/assets/hostvm-install-3.jpg)

3. Перейдите на вкладку Сеанс, нажмите кнопку `Сохранить`, нажмите клавишу `Enter` чтобы запустить сессию:

![](../.gitbook/assets/hostvm-install-4.jpg)

### Проверить, что диск предназначенный для размещения виртуальных машин подключен

Командой `cat /etc/fstab` выведите на экран список используемых в системе устройств хранения. В качестве точки монтирования мы использовали директорию `/data`.

```text
[root@host1 ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Fri Nov  1 09:31:43 2019
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos_host1-root /                       xfs     defaults        0 0
UUID=2412661b-df41-46c7-ad31-b5b696dfc218 /boot                   xfs     defaults        0 0
/dev/mapper/centos_host1-data /data                   xfs     defaults        0 0
/dev/mapper/centos_host1-swap swap                    swap    defaults        0 0
```

### Настройка прокси \(если используется\)

Если в данной сети доступ во вне доступен только через прокси, то следует выполнить следующие настройки:

1. В файле /etc/yum.conf, для загрузки пакетов через прокси добавляем строки

```text
proxy=http://proxyhost:8080
proxy_username=proxyname
proxy_password=proxypass
```

   2. Экспортировать глобальные переменные прокси для работы CURL

```text
## http прокси с именем и паролем 
export http_proxy=http://user:password@your-proxy-ip-address:port/

## HTTPS версия ##
export https_proxy=https://your-ip-address:port/
export https_proxy=https://user:password@your-proxy-ip-address:port/
```

Кроме этого существует опция -x для команды curl что бы указать прокси сервер

```text
curl -x <[protocol://][user:password@]proxyhost[:port]> url
--proxy <[protocol://][user:password@]proxyhost[:port]> url
--proxy http://user:password@Your-Ip-Here:Port url
-x http://user:password@Your-Ip-Here:Port url
```

Например, сначала устанавливаем переменные для прокси и затем используем CURL

```text
export http_proxy=http://foo:bar@1.1.1.1:3128/
export https_proxy=$http_proxy
## Use curl command ##
curl -I www.system-admins.ru
curl -v -I www.system-admins.ru
```

### Загрузка и выполнение подготовительного скрипта `initial.sh`

С помощью команды `curl` выполните загрузку файла `initial.sh` с репозитория repo.hostco.ru

```text
[root@host1 ~]# curl -o /root/initial.sh https://repo.hostco.ru/hostvm/initial.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1192  100  1192    0     0   7791      0 --:--:-- --:--:-- --:--:--  7842
```

Cделайте файл `initial.sh` исполняемым

```text
[root@host1 ~]# chmod +x initial.sh 
[root@host1 ~]# ls 
anaconda-ks.cfg  initial.sh  original-ks.cfg
[root@host1 ~]# ls -la
total 40
dr-xr-x---.  2 root root  155 Apr  7 09:31 .
dr-xr-xr-x. 18 root root  255 Apr  7 08:36 ..
-rw-r--r--.  1 root root   18 Dec 29  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 29  2013 .bash_profile
-rw-r--r--.  1 root root  176 Dec 29  2013 .bashrc
-rw-r--r--.  1 root root  100 Dec 29  2013 .cshrc
-rw-r--r--.  1 root root  129 Dec 29  2013 .tcshrc
-rw-------.  1 root root 5570 Jun  1  2019 anaconda-ks.cfg
-rwxr-xr-x.  1 root root  169 Apr  7 09:32 initial.sh
-rw-------.  1 root root 5300 Jun  1  2019 original-ks.cfg
[root@host1 ~]#
```

Выполните скрипт `initial.sh`

```text
[root@host1 ~]# sh initial.sh
```

По итогу выполнения скрипта, в директории /root/ будет создан лог-файл `initial_log_текущая-дата.log`

## Заполнение формы для установки значений переменных

### Сбор данных для заполнения формы

Перед началом работы рекомендуется заполнить последний столбец следующей таблицы \(_способ сбора данных для таблице описан ниже по тексту\)_:

| Название | Как узнать | Значение |
| :--- | :---: | :---: |
| ip для engine | - |  |
| ip сервера | ip addr |  |
| ip шлюза по умолчанию | ip route |  |
| ip dns-сервера | - |  |
| домен установки | - |  |
| hostname сервера | - |  |
| название интерфейса | ip addr |  |
| предпочтительный gluster-hostname | - |  |
| предпочтительное название тома gluster | - |  |
| директорию для размещения тома gluster | - |  |

Для получения ip-адреса сервера и название интерфейса выполните команду `ip addr` :

Согласно примеру ниже видно, что ip-адрес сервера - `10.1.140.13`, название интерфейса - `enp3s0`.

```text
[root@host1 ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:17:a4:77:00:0c brd ff:ff:ff:ff:ff:ff
    inet 10.1.140.13/25 brd 10.1.140.127 scope global noprefixroute enp3s0
       valid_lft forever preferred_lft forever
    inet6 fe80::b50a:7c22:2229:b169/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
3: enp7s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:17:a4:77:00:0e brd ff:ff:ff:ff:ff:ff
```

Для получения ip-адреса шлюза выполните команду `ip route`.

Согласно примеру ниже видно, что ip шлюза по умолчанию - `10.1.140.1`

```text
[root@host1 ~]# ip route
default via 10.1.140.1 dev enp3s0 proto static metric 100
10.1.140.0/25 dev enp3s0 proto kernel scope link src 10.1.140.13 metric 100
```

Для параметры связанные с glusterfs могут быть выбраны значения по умолчанию:

| Название | Значение по умолчанию |
| :--- | :---: |
| предпочтительный gluster-hostname | glusternode1 |
| предпочтительное название тома gluster | hosted-engine |
| директорию для размещения тома gluster | /data/gluster/hosted\_engine |

Обратите внимание, что рекомендуется использовать отдельный раздел для размещения тома glusterfs, который имеет точку монтирования в директории _/data_

### Запуск программы-помощника IP-wizard

Запустите `IP-wizard.sh`, чтобы подготовить файлы переменных к работе. Следуйте указаниями инструкции в программе:

```text
sh IP-wizard.sh 

Добро пожаловать в программу-помощник IP-wizard Группы компаний ХОСТ!
Мы попросим ответить на несколько вопросов и сформируем нужные файлы конфигурации Ansible.

Внимание! Программа изменит файлы в папках /etc/ansible/group_vars и /etc/ansible/host_vars!

Нажмите ENTER для продолжения или ^C для выхода из программы!
Для принятия значений по-умолчанию просто нажимайте ENTER. ;)

Укажите ваш Домен: mydomain.ru
Домен: mydomain.ru

Укажите кластерный IP адрес oVirt Engine: 10.1.140.15
Engine: 10.1.140.15

Укажите маску сети для кластерного IP адреса oVirt Engine(в виде числа: 24, 25 и т.д. (т.е. /24 /25 т.д.)): 25
EngineMask: 24

Укажите DNS имя для консоли управления(oVirt Engine)(без домена) : engine-test
hostname консоли управления: engine-test

Укажите доступный клиентам IP адрес текущего сервера  : 10.1.140.13
nodeip1: 10.1.140.13

Укажите шлюз (gateway) сети текущего сервера : 10.1.140.1
Public LAN gateway: 10.1.140.1

Укажите hostname первого сервера (без домена): myhost1
hostname первого сервера: myhost1

На сервере будет развернута нода glusterfs.
Укажите предпочтительный gluster-hostname первого сервера (без домена)(glusternode1): glust1
gluster-hostname первого сервера: glust1

В среде виртуализации для размещения управляющей машины engine будет создан домен хранения hosted-engine
Укажите предпочтительное название тома gluster (hosted-engine): 
Имя тома gluster для домена хранения hosted-engine: hosted-engine

Укажите директорию для размещения тома gluster hosted-engineОбратите внимание, что для установки на разделе выбранного расположения директории должно быть свободно минимум 61ГБ
(/data/gluster/hosted_engine): 
Директория для glusterfs : /data/gluster/hosted_engine

Укажите hostname имя интерфейса первого сервера (Например enp2s0f0. Можно посмотреть командой ip addr): enp2s0f0
имя интерфейса первого сервера: enp2s0f0


Укажите ваш DNS сервер: 10.1.64.248
DNS: 10.1.64.248

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

```text
[root@host1 ~]# ansible-playbook /etc/ansible/make-prepare.yml
```

Выполните команду `ansible-playbook /etc/ansible/make-gluster-storages.yml`, чтобы подготовить к работе glusterfs.

```text
[root@host1 ~]# ansible-playbook /etc/ansible/make-gluster-storages.yml
```

Запустите установку необходимых пакетов виртуализации командой `ansible-playbook /etc/ansible/make-ovirt.yml`. На ее выполнение уйдет чуть больше часа.

```text
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

```text
/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log
```

После завершения развертывания виртуализации откройте браузер и перейдите по адресу [https://engine.mydomain.ru](https://engine.mydomain.ru), чтобы попасть в панель управления \(Адрес может отличаться, если Вы задали другое DNS имя консоли управления\).

## Если что-то пошло не так

1. Проверить корректность данных, которые были введены в IP-wizard. При обнаружении ошибки выполните команду `ansible-playbook /etc/ansible/clean-node.yml` и начните сначала.
2. Если на этапе `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` появилась ошибка, то выполните команду `ansible-playbook /etc/ansible/clean-node.yml` и начните сначала
3. Если на этапе `ansible-playbook /etc/ansible/make-prepare.yml` появилась ошибка, повторите выполнение данной команды
4. Если на этапе `ansible-playbook /etc/ansible/make-gluster-storages.yml` появилась ошибка, повторите выполнение данной команды
5. Если на этапе `ansible-playbook /etc/ansible/make-ovirt.yml` появилась ошибка, повторите выполнение данной команды
6. Если после завершения установки вам не открывается страница в браузере с адресом [https://engine.mydomain.ru](https://engine.mydomain.ru), то
   1. Проверьте, что ip для engine, указанный в таблице в начале установки отвечает на команду ping
   2. Проверьте, что имя `engine.mydomain.ru` разрешается вашим dns-сервером.
7. Если на этапе установки engine `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` установка зависает на этапе `Engine VM domain: [rtc.local]rtc.local Enter root password that will be used for the engine appliance: engine`, то подключитесь к консоли сервера не по SSH, а с помощью ipmi\(iLO, iDRAC, etc.\) и повторно запустите скрипт установки engine.

Схема установки hostvm и самостоятельного решения проблем представлена на рисунке ниже:

![](../.gitbook/assets/troubleshooting-scheme-on-local-disks.jpg)

Если устранить проблему не удалось, обратитесь в [техническую поддержку](https://github.com/hostco-ru/hostvm/tree/1e76c2e8efd1596a96107b99841d3e81adefe102/docs/hostco.ru) используя [инструкцию](https://reestr.hostco.ru/downloads) К обращению приложите лог вывода вашей консоли, который был настроен в начале установки и файл `/root/script-hosted-engine-deploy.log`.

