# Установка HOSTVM Manager на Gluster

В случае, если установка производится без подключения к Интернету, то необходимо скачать из Личного Кабинета ISO-образ локального репозитория: hostvm-node-ng-installer-local-repo и заменить установочный диск на него.\
После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root. Альтернативно можно подключиться через [putty](https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/podgotovka-putty-k-rabote).

Пароль root для hostvm node по умолчанию: HostvmNode.

Логин HOSTVM Manager по умолчанию: admin\
Пароль HOSTVM Manager задается в процессе установки, по умолчанию HostvmManager

Перейдите на вкладку Terminal

<figure><img src="https://lh4.googleusercontent.com/08uXFXMDZkrjbaFWCA2Xj_vlAeKa36WdLKmUvMzZYENG_UldWbw4fhV26xQw8fX11kMMEzk27cPyXyTf-yNPtSesTV3b13FBX02tYr3cvH7V6EvGTMWxPPs9yxHIyhp9N_7yMLWj9ArwET1h3L5M8HI" alt=""><figcaption></figcaption></figure>

## Запуск программы-помощника IP-wizard

Запустите `IP-wizard.sh`, чтобы подготовить файлы переменных к работе. Следуйте указаниями инструкции в программе:

```
sh IP-wizard.sh 

Добро пожаловать в программу-помощник IP-wizard HOSTVM!
Мы попросим ответить на несколько вопросов и сформируем нужные файлы конфигурации Ansible.

Внимание! Программа изменит файлы в папках /etc/ansible/group_vars и /etc/ansible/host_vars!

Нажмите ENTER для продолжения или ^C для выхода из программы!
Для принятия значений по-умолчанию просто нажимайте ENTER. ;)

Укажите ваш Домен: pvhostvm.ru
Домен: pvhostvm.ru

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
Введите пароль для пользователя admin веб-интефейса HOSTVM Manager:
Введите пароль root для управляющей машины:
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
ovirt_engine_password: 'HostvmManager'
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

```

## Установка виртуализации

Выполните команду `ansible-playbook /etc/ansible/make-prepare.yml`, чтобы подготовить к работе /etc/hosts.

```
[root@host1 ~]# ansible-playbook /etc/ansible/make-prepare.yml
```

Выполните команду `ansible-playbook /etc/ansible/make-gluster-storages.yml`, чтобы подготовить к работе glusterfs.

```
[root@host1 ~]# ansible-playbook /etc/ansible/make-gluster-storages.yml
```

Запустите установку необходимых пакетов виртуализации командой `ansible-playbook /etc/ansible/make-ovirt.yml`. На ее выполнение уйдет чуть больше часа.

```
[root@host1 ~]# ansible-playbook /etc/ansible/make-ovirt.yml
```

Сформированный файл `/root/script-hosted-engine-deploy` содержит инструкции, необходимые для развертывания виртуализации.\
Запустите его на исполнение командой `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log`:

```
/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log
```

## Если что-то пошло не так

1\. В случае, если возникают ошибки, связанные с доступом к локальному репозиторию (local-repo) выполните следующую команду:

```
systemctl restart systemd-udevd
```

Либо перезагрузите хост.\
2\. Проверить корректность данных, которые были введены в IP-wizard. При обнаружении ошибки выполните команду ansible-playbook /etc/ansible/clean-node.yml и начните сначала.

3.Если на этапе `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` появилась ошибка, то выполните команду `ansible-playbook /etc/ansible/clean-node.yml` и начните сначала

4.Если на этапе `ansible-playbook /etc/ansible/make-prepare.yml` появилась ошибка, повторите выполнение данной команды

5.Если на этапе `ansible-playbook /etc/ansible/make-gluster-storages.yml` появилась ошибка, повторите выполнение данной команды

6.Если на этапе `ansible-playbook /etc/ansible/make-ovirt.yml` появилась ошибка, повторите выполнение данной команды

7.Если после завершения установки вам не открывается страница в браузере с адресом `https://engine.mydomain.ru`, то

* Проверьте, что ip для HOSTVM Manager, указанный в таблице в начале установки отвечает на команду ping
* Проверьте, что имя engine.mydomain.ru разрешается вашим dns-сервером.

8. Если на этапе установки engine `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` установка зависает на этапе `Engine VM domain: [rtc.local]rtc.local Enter root password that will be used for the engine appliance: engine`, то подключитесь к консоли сервера не по SSH, а с помощью ipmi(iLO, iDRAC, etc.) и повторно запустите скрипт установки менеджера.

Если устранить проблему не удалось, обратитесь в[ техническую поддержку](https://lk.pvhostvm.ru/) используя[ инструкцию](https://lk.pvhostvm.ru/) К обращению приложите лог вывода вашей консоли, который был настроен в начале установки и файл `/root/script-hosted-engine-deploy.log.`

\
<br>
