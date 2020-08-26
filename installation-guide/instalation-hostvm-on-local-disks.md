# Установка виртуализации на локальные диски

## Подготовка сервера к развертыванию на локальных дисках

### Подготовка putty к работе

Убедитесь, что требования, описанные на странице [Системные требования](requirements.md) выполняются.

С помощью программы [PuTTY](https://www.putty.org), которая доступна в [наборе дистрибьютивов для развертывания решения](https://reestr.hostco.ru/downloads), под пользователем root подключитесь к серверу.

Перед началом работы рекомендуется настроить логирование сессии putty. Для этого нужно выполнить следующие действия

1. Сохраните имя сервера:

![](../.gitbook/assets/screenshot_31.png)

2. Перейдите на вкладку Журнал, выберите `Весь вывод`, укажите путь до файла логов в следующем виде: `C:\path\to\log\hostname-&H-&Y&M&D-&T.log`. Часть `&H-&Y&M&D-&T` указывает, что файл с логом будет создаваться для каждой сессии и автоматически указывать время и дату ее начала:

![](../.gitbook/assets/screenshot_32.png)

3. Перейдите на вкладку Сеанс, нажмите кнопку `Сохранить`, нажмите клавишу `Enter` чтобы запустить сессию:

![](../.gitbook/assets/screenshot_33.png)

### Проверить, что диск предназначенный для размещения виртуальных машин подключен

Командой `cat /etc/fstab` выведите на экран список используемых в системе устройств хранения. В качестве точки монтирования мы использовали директорию `/data`.

```text
[root@virt2 ~]# cat /etc/fstab
#
# /etc/fstab
# Created by anaconda on Wed Aug 26 13:30:05 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/hn_virt2/ovirt-node-ng-4.3.9-0.20200319.0+1 / ext4 defaults,discard 1 1
UUID=4dfc5afd-299a-40fa-94bb-f65820e77325 /boot                   ext4    defaul                                                                                ts        1 2
/dev/mapper/hn_virt2-data /data                   ext4    defaults        1 2
/dev/mapper/hn_virt2-var /var ext4 defaults,discard 1 2
/dev/mapper/hn_virt2-swap swap                    swap    defaults        0 0
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

На примере, сначала устанавливаем переменные для прокси и затем используем CURL

```text
export http_proxy=http://foo:bar@1.1.1.1:3128/
export https_proxy=$http_proxy
## Use curl command ##
curl -I www.system-admins.ru
```

Выполните скрипт `initial.sh`

```text
[root@virt2 ~]# sh initial.sh
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

Согласно примеру ниже видно, что ip-адрес сервера - `10.1.158.140`, название интерфейса - `eno1`.

```text
[root@virt2 ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defaul                                                                                t qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp8s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group                                                                                 default qlen 1000
    link/ether 5c:f3:fc:11:42:90 brd ff:ff:ff:ff:ff:ff
3: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group defa                                                                                ult qlen 1000
    link/ether 5c:f3:fc:11:42:91 brd ff:ff:ff:ff:ff:ff
    inet 10.1.158.140/24 brd 10.1.158.255 scope global noprefixroute eno1
       valid_lft forever preferred_lft forever
    inet6 fe80::e54:1202:33cf:5a8a/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
4: enp0s29u1u1u5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast st                                                                                ate UNKNOWN group default qlen 1000
    link/ether 5e:f3:fc:15:16:bb brd ff:ff:ff:ff:ff:ff
    inet 169.254.95.120/24 brd 169.254.95.255 scope link noprefixroute dynamic e                                                                                np0s29u1u1u5
       valid_lft 346sec preferred_lft 346sec
    inet6 fe80::42ed:eae9:47ac:6b89/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

Для получения ip-адреса шлюза выполните команду `ip route`.

Согласно примеру ниже видно, что ip шлюза по умолчанию - `10.1.158.1`

```text
[root@virt2 ~]# ip route
default via 10.1.158.1 dev eno1 proto static metric 100
10.1.158.0/24 dev eno1 proto kernel scope link src 10.1.158.140 metric 100
169.254.95.0/24 dev enp0s29u1u1u5 proto kernel scope link src 169.254.95.120 metric 101
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
[root@virt2 ~]# sh IP-wizard.sh

Добро пожаловать в программу-помощник IP-wizard Группы компаний ХОСТ!
Мы попросим ответить на несколько вопросов и сформируем нужные файлы конфигурации Ansible.

Внимание! Программа изменит файлы в папках /etc/ansible/group_vars и /etc/ansible/host_vars!

Нажмите ENTER для продолжения или ^C для выхода из программы!
Для принятия значений по-умолчанию просто нажимайте ENTER. ;)

Укажите ваш Домен: mydomain.ru
Домен: mydomain.ru

Укажите кластерный IP адрес oVirt Engine: 10.1.158.141
Engine: 10.1.158.141


Укажите маску сети для кластерного IP адреса oVirt Engine(в виде числа: 24, 25 и т.д. (т.е. /24 /25 т.д.)): 24
EngineMask: 24

Укажите DNS имя для консоли управления(oVirt Engine)(без домена) : engine
hostname консоли управления: engine

Укажите доступный клиентам IP адрес текущего сервера  : 10.1.158.140
nodeip1: 10.1.158.140

Укажите шлюз (gateway) сети текущего сервера : 10.1.158.1
Public LAN gateway: 10.1.158.1

Укажите hostname текущего хоста (без домена) : virt2
hostname текущего сервера: virt2

На сервере будет развернута нода glusterfs.
Укажите предпочтительный gluster-hostname первого сервера (без домена) (glusternode1):
gluster-hostname первого сервера: glusternode1

В среде виртуализации для размещения управляющей машины engine будет создан домен хранения hosted-engine
Укажите предпочтительное название тома gluster  (hosted-engine):
Имя тома gluster для домена хранения hosted-engine: hosted-engine

Укажите директорию для размещения тома gluster hosted-engine
Обратите внимание, что для установки на разделе выбранного расположения директории должно быть свободно минимум 61ГБ
(/data/gluster/hosted_engine):
Директория для glusterfs : /data/gluster/hosted_engine

Укажите hostname имя интерфейса первого сервера (Например enp2s0f0. Можно посмотреть командой ip addr) : eno1
имя интерфейса первого сервера: eno1


Укажите ваш DNS сервер : 10.1.64.254
DNS: 10.1.64.254
Начинаю модификацию файлов...

dns_root: 10.1.64.254
Изменен файл /etc/ansible/group_vars/all.

Генерируем /etc/ansible/group_vars/nodes...
ansible_connection: ssh
ansible_ssh_user: root
ansible_ssh_pass: engine
ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
ovirt_engine_ip: 10.1.158.141
ovirt_engine_fqdn: 'engine.mydomain.ru'
ovirt_engine_domain: 'mydomain.ru'
ovirt_engine_password: 'engine'
Изменен файл /etc/ansible/group_vars/nodes.

Генерируем /etc/ansible/host_vars/host1...
hostname: virt2.mydomain.ru
shothostname: virt2
ip: 10.1.158.140
ip_gateway: 10.1.158.1
nic_for_ovirtmgmt_bridge: eno1
gluster_hostname: glusternode1
Изменен файл /etc/ansible/host_vars/host1.

Генерируем /etc/ansible/group_vars/gluster...
gluster_dir_for_hosted_engine: /data/gluster/hosted_engine/brick1
gluster_hosted_engine_volume_name: hosted-engine
Изменен файл /etc/ansible/group_vars/gluster.

Правим файл ответов для hosted-engine...
Файл ответов обновлен.
[root@virt2 ~]#

```

## Установка виртуализации

Выполните команду `ansible-playbook /etc/ansible/make-prepare.yml`, чтобы подготовить к работе /etc/hosts.

```text
[root@virt2 ~]# ansible-playbook /etc/ansible/make-prepare.yml
```

Выполните команду `ansible-playbook /etc/ansible/make-gluster-storages.yml`, чтобы подготовить к работе glusterfs.

```text
[root@virt2 ~]# ansible-playbook /etc/ansible/make-gluster-storages.yml
```

Запустите установку необходимых пакетов виртуализации командой `ansible-playbook /etc/ansible/make-ovirt.yml`. На ее выполнение уйдет чуть больше часа.

```text
[root@virt2 ~]# ansible-playbook /etc/ansible/make-ovirt.yml
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

[root@virt2 ~]#
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

