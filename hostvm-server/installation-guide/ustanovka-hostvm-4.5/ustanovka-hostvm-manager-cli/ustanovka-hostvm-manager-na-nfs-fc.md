# Установка HOSTVM Manager на NFS/FC

В случае, если установка производится без подключения к Интернету, то необходимо скачать из Личного Кабинета ISO-образ локального репозитория: hostvm-node-ng-installer-local-repo и заменить установочный диск на него.\
После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root. Альтернативно можно подключиться через [putty](https://kb.pvhostvm.ru/hostvm/installation-guide/ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/podgotovka-putty-k-rabote).

Пароль root для hostvm node по умолчанию: HostvmNode.

Логин HOSTVM Manager по умолчанию: admin\
Пароль HOSTVM Manager задается в процессе установки, по умолчанию HostvmManager

Перейдите на вкладку Terminal

<figure><img src="https://lh4.googleusercontent.com/08uXFXMDZkrjbaFWCA2Xj_vlAeKa36WdLKmUvMzZYENG_UldWbw4fhV26xQw8fX11kMMEzk27cPyXyTf-yNPtSesTV3b13FBX02tYr3cvH7V6EvGTMWxPPs9yxHIyhp9N_7yMLWj9ArwET1h3L5M8HI" alt=""><figcaption></figcaption></figure>

На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины HOSTVM Manager, которая будет установлена.\
Если на DNS-сервере отсутствуют записи, то их можно добавить вручную на ноде HOSTVM: [**Действия при установке HOSTVM при отсутствии записей в DNS**](https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns)

{% hint style="warning" %}
**В случае установки на FC**, диск для организации хранилища виртуальных машин должен отвечать следующим требованиям:\


* объем диска – не менее 80 Gb;
* диск должен быть пустым и не содержать в себе какой-либо файловой системы;
* диск не может быть примонтирован к текущей файловой системе.
{% endhint %}

{% hint style="warning" %}
**В случае установки на NFS**, объём NFS-хранилища должен быть не менее 80 Гб.
{% endhint %}

\
**Процедура**
-------------

1\. Используйте оконный менеджер tmux для запуска скрипта, чтобы избежать потери сеанса в случае сбоя сети или терминала.

Установите и запустите tmux:

```
# dnf -y install tmux
# tmux
```

\
2\. Запустите установку управляющей машины:

```
[root@testname1 ~]# hosted-engine --deploy --4 | tee -a /root/hosted-engine-deploy.log
```

3\. Выберите Yes, чтобы начать установку:

```
Continuing will configure this host for serving as hypervisor and will create a local VM with a running engine.
The locally running engine will be used to configure a new storage domain and create a VM there.
At the end the disk of the local VM will be moved to the shared storage.
Are you sure you want to continue? (Yes, No)[Yes]:
```

4\. Настройте сеть. Убедитесь, что шлюз указан правильно, и нажмите Enter.

```
Please indicate the gateway IP address [X.X.X.X]:
```

5\. Скрипт определяет возможные сетевые адаптеры для использования в качестве моста управления для среды. Введите один из них или нажмите Enter, чтобы принять значение по умолчанию

```
Please indicate a nic to set ovirtmgmt bridge on (ens3) [ens3]: ens3
```

6\. Укажите тип проверки подключения к сети:  &#x20;

```
Please specify which way the network connectivity should be  checked (ping, dns, tcp, none) [dns]: ping
```

\
7\. Укажите название датацентра. Название по умолчанию Default.

```
Please enter the name of the data center where you want to deploy this hosted-engine host.
Data center [Default]:  
```

&#x20;         \
8\. Укажите название кластера. Название по умолчанию Default.&#x20;

```
Please enter the name of the cluster where you want to deploy this hosted-engine host.
Cluster [Default]: Default
```

\
9\. Выберите, нужна ли настройка Keycloak, его можно будет настроить позднее (ссылка на статью)

```
Configure Keycloak integration on the engine(Yes, No) [Yes]: No
```

\
10\. Введите количество ядер CPU и оперативной памяти, которые будут выделены управляющей виртуальной машине:

```
Please specify the number of virtual CPUs for the VM. The default is the appliance OVF value [4]:
Please specify the memory size of the VM in MB. The default is the maximum available [14541]:
```

11\. Укажите FQDN для управляющей виртуальной машины:

```
Please provide the FQDN you would like to use for the engine.
Note: This will be the FQDN of the engine VM you are now going to launch, it should not point to the base host or to any other existing machine.
Engine VM FQDN: engine-454
```

12\. Укажите домен для управляющей виртуальной машины:

```
Please provide the domain name you would like to use for the engine appliance.
Engine VM domain [pvhostvm.ru]: pvhostvm.ru
```

\
13\. Укажите пароль для пользователя root управляющей машины:

```
Enter root password that will be used for the engine appliance:
Confirm appliance root password:
```

14\. Опционально: введите публичный ssh-ключ для подключения к управляющей виртуальной машине как пользователь root без пароля. Укажите, следует ли включать доступ по ssh для пользователя root:

```
You may provide an SSH public key, that will be added by the deployment script to the authorized_keys file of the root user in the engine appliance.
This should allow you passwordless login to the engine machine after deployment.
If you provide no key, authorized_keys will not be touched.
SSH public key []:
Do you want to enable ssh access for the root user (yes, no, without-password) [yes]:
```

15\. Укажите, следует ли применить профиль OpenSCAP:

```
Do you want to apply an OpenSCAP security profile? (Yes, No)
[No]: No
```

16\. Введите MAC адрес для управляющей виртуальной машины или примите случайно сгенерированный. Если вы хотите предоставить управляющей виртуальной машине IP-адрес через DHCP, убедитесь, что у вас есть действительное резервирование DHCP для этого MAC-адреса. Сценарий развертывания не будет настраивать DHCP-сервер для вас.

```
Please specify a unicast MAC address for the VM, or accept a randomly generated default [00:16:3e:0e:c5:22]: 
```

17\. Укажите способ сетевой конфигурации:

```
How should the engine VM network be configured?
(DHCP,Static)[DHCP]: 

```

Если вы выбрали Static, введите IP-адрес управляющей виртуальной машины:

{% hint style="warning" %}
**Важно**

·        Статичный IP-адрес должен быть в одной подсети с хостом. Например, если IP хоста - 10.1.1.0/24, IP управляющей виртуальной машины должен быть в диапазоне этой подсети (10.1.1.1-254/24).

·         Для IPv6, поддерживается только статическая адресация.
{% endhint %}

```
Please enter the IP address to be used for the engine VM []: 
Please provide a comma-separated list (max 3) of IP addresses of domain name servers for the engine VM
Engine VM DNS (leave it empty to skip) [X.X.X.X]: 
```

18\. Укажите, следует ли добавлять записи для управляющей виртуальной машины и базового хоста в файл /etc/hosts виртуальной машины. Вы должны убедиться, что имена хостов разрешимы.&#x20;

```
Add lines for the appliance itself and for this host to /etc/hosts on the engine VM?
Note: ensuring that this host could resolve the engine VM hostname is still up to you.
Add lines to /etc/hosts? (Yes, No)[Yes]: Yes
```

19\. Укажите имя и номер TCP-порта SMTP-сервера, адрес электронной почты, используемый для отправки уведомлений по электронной почте, и разделенный запятыми список адресов электронной почты для получения этих уведомлений. Или нажмите Enter, чтобы принять значения по умолчанию:

```
Please provide the name of the SMTP server through which we will send notifications [localhost]:
Please provide the TCP port number of the SMTP server [25]:          
Please provide the email address from which notifications will be sent [root@localhost]:
Please provide a comma-separated list of email addresses which will get notifications [root@localhost]:
```

20\. Задайте пароль для пользователя admin@internal для доступа к порталу администрирования и подтвердите его:

```
Enter engine admin password:
Confirm engine admin password:
```

21\. Укажите hostname для этого хоста:

```
Please provide the hostname of this host on the management network [hostname.example.com]:
```

Скрипт создаст виртуальную машину. По умолчанию, скрипт сначала скачивает и устанавливает engine appliance, что увеличивает время установки.

22\. Укажите тип хранилища (fc или nfs):

```
Please specify the storage you would like to use (glusterfs,iscsi, fc, nfs)[nfs]:
```

*   Для NFS выберите версию, адрес и путь подключения к хранилищу, а также параметры монтирования:\


    ```
    Please specify the nfs version you would like to use (auto, v3, v4, v4_1)[auto]:
    Please specify the full shared storage connection path to use (example: host:/path): storage.example.com:/hosted_engine/nfs
    If needed, specify additional mount options for the connection to the hosted-engine storage domain []:
    ```
* Для FC выберите LUN из списка автоматического обнаружения. LUN не должен cодержать существующих данных.

```
The following luns have been found on the requested target:
[1] 3514f0c5447600351   30GiB   XtremIO XtremApp
         status: used, paths: 2 active
 
[2] 3514f0c5447600352   30GiB   XtremIO XtremApp
         status: used, paths: 2 active
 
Please select the destination LUN (1, 2) [1]:
```

\
23\. Укажите размер диска управляющей виртуальной машины:

```
Please specify the size of the VM disk in GiB: [51]:
```

После завершения развертывания виртуализации откройте браузер и перейдите по адресу `https://engine.mydomain.ru`, чтобы попасть в панель управления.

## **Если что-то пошло не так**

1\. В случае, если возникают ошибки, связанные с доступом к локальному репозиторию (local-repo) выполните следующую команду:

```
systemctl restart systemd-udevd
```

Либо перезагрузите хост.\
2\. Проверьте корректность вводимых данных в сценарии развёртывания. При обнаружении ошибок выполните команду `/usr/sbin/ovirt-hosted-engine-cleanup`, очистите хранилище и начните сначала.

3\. Если после завершения установки не открывается страница в браузере с адресом `https://engine.mydomain.ru`, то

* проверьте, что Ip для управляющей машины, указанный в таблице в начале установки, отвечает на команду ping;
* проверьте, что имя engine.mydomain.ru разрешается вашим dns-сервером.

Если устранить проблему не удалось, обратитесь в[ техническую поддержку](https://lk.pvhostvm.ru/) используя[ инструкцию](https://lk.pvhostvm.ru/) К обращению приложите логи установки, которые находятся в директории `/var/log/ovirt-hosted-engine-setup`.

\
