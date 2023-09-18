# Обновление Hosted HOSTVM Manager

**Для обновления Hosted HOSTVM Manager необходимо соблюсти следующие условия:**

* Обновлять HOSTVM Manager 4.4.\* -> 4.5.\* можно только на хосте с ОС CentOS 8.x (HOSTVM Node 4.5.\*)
* С хоста, на котором будет проводиться обновление, необходимо смигрировать все ВМ на другие хосты кластера
* Подготовить дополнительное хранилище размером минимум 70 Гб, для размещения ВМ Hosted HOSTVM Manager 4.5.\* (при восстановлении конфигурации потребуется указать Storage Domain, отличный от того, что использовался на версии 4.4.\*)
* На время обновления отключить fencing на хостах, через портал администрирования:

`Compute -> Clusters -> Выделить нужный кластер -> Edit -> Fencing policy -> Снять галку Enable fencing`

{% hint style="info" %}
Во время обновления запущенные ВМ могут продолжать работу\
**Портал администрирования во время обновления будет недоступен**
{% endhint %}

**Установка обновления**

1\) С целевого хоста, на котором будет проводиться обновление, смигрировать все ВМ, включая HOSTVM Manager, на другие хосты кластера

2\) Вывести обновляемый хост из кластера, установить на него hostvm-node 4.5.\*, ввести обратно в кластер согласно[ инструкции по обновлению HOSTVM Node](obnovlenie-hostvm-node.md)​

3\) Проверить что статус хоста - "ОК" :

`nodectl check`

{% hint style="info" %}
Дальнейшие действия сделают недоступным портал администрирования

**Необходимо отключить fencing на обновляемых хостах**
{% endhint %}

`Compute -> Clusters -> Выделить нужный кластер -> Edit -> Fencing policy -> Снять галку Enable fencing`\
4\)Подключиться по SSH на хост, на котором запущена ВМ с Hosted HOSTVM Manager

5\) Выполнить `hosted-engine --set-maintenance --mode=global`

6\) Убедиться, что кластер перешел в режим global maintenance:

`hosted-engine --vm-status`

7\) Подключиться по SSH к ВМ Hosted HOSTVM Manager

8\) Остановить сервис oVirt Engine: `systemctl stop ovirt-engine`

9\) Сделать резервную копию конфигурации управляющей машины

`engine-backup --scope=all --mode=backup --file=backup.bck --log=backuplog.log`

10\) Загрузить файл резервной копии конфигурации на обновленный в п.2 хост, на котором будет развертываться Hosted HOSTVM Manager 4.5.\*

11\) Остановить управляющую ВМ 4.4.\*

`hosted-engine --vm-shutdown`

12\) Подключиться по SSH к хосту с Hostvm Node 4.5.\*, запустить процесс развертывания Hosted HOSTVM Manager 4.5.\*, с восстановлением конфигурации из резервной копии:

`hosted-engine --deploy --4 --restore-from-file=backup.bck`

**Пример ответов на вопросы инсталлятора**

Настройка [Keycloak](../ustanovka-keycloak-hostvm-4.5.md) осуществляется после обновления HOSTVM Manager и всех хостов в кластере до версии 4.5.\*

```
* Please note * : Keycloak is now deprecating AAA/JDBC authentication module.
It is highly recommended to install Keycloak based authentication.
Configure Keycloak on this host (Yes, No) [Yes]: No
```

Пропущенные вопросы оставить по умолчанию:

```
Continuing will configure this host for serving as hypervisor and will create a local VM with a running engine.
The provided engine backup file will be restored there,
it's strongly recommended to run this tool on an host that wasn't part of the environment going to be restored.
If a reference to this host is already contained in the backup file, it will be filtered out at restore time.
The locally running engine will be used to configure a new storage domain and create a VM there.
At the end the disk of the local VM will be moved to the shared storage.
The old hosted-engine storage domain will be renamed, after checking that everything is correctly working you can manually remove it.
Other hosted-engine hosts have to be reinstalled from the engine to update their hosted-engine configuration.
Are you sure you want to continue? (Yes, No)[Yes]: Yes
[ INFO ] Bridge ovirtmgmt already created
Please indicate the gateway IP address [10.1.158.1]: 10.1.158.1
[ INFO ] TASK [ovirt.hosted_engine_setup : Validate selected bridge interface if management bridge does not exist]
[ INFO ] skipping: [localhost]
Please indicate a nic to set ovirtmgmt bridge on: (enp8s0f0) [enp8s0f0]: enp8s0f0
Please specify which way the network connectivity should be checked (ping, dns, tcp, none) [dns]: ping
```

Наименования Datacenter и Cluster должны быть отличны от изначальных

```
--== VM CONFIGURATION ==--
​
Please enter the name of the datacenter where you want to deploy this hosted-engine host. Please note that if you are restoring a backup that contains info about other hosted-engine hosts,
this value should exactly match the value used in the environment you are going to restore. [Default]: Default
Please enter the name of the cluster where you want to deploy this hosted-engine host. Please note that if you are restoring a backup that contains info about other hosted-engine hosts,
this value should exactly match the value used in the environment you are going to restore. [Default]: Default
​
Renew engine CA on restore if needed? Please notice that if you choose Yes, all hosts will have to be later manually reinstalled from the engine. (Yes, No)[No]: No
​
Pause the execution after adding this host to the engine?
You will be able to iteratively connect to the restored engine in order to manually review and remediate its configuration before proceeding with the deployment:
please ensure that all the datacenter hosts and storage domain are listed as up or in maintenance mode before proceeding.
This is normally not required when restoring an up to date and coherent backup. (Yes, No)[No]: No
​
...
​
Please specify the memory size of the VM in MB (Defaults to appliance OVF value): [16384]: 12000
[ INFO ] Detecting host timezone.
Please provide the FQDN you would like to use for the engine.
Note: This will be the FQDN of the engine VM you are now going to launch,
it should not point to the base host or to any other existing machine.
Engine VM FQDN: []: engine1.mydomain.ru
Please provide the domain name you would like to use for the engine appliance.
Engine VM domain: [mydomain.ru] mydomain.ru
​
...
​
How should the engine VM network be configured (DHCP, Static)[DHCP]? Static
Please enter the IP address to be used for the engine VM []: 10.1.158.141
Please provide a comma-separated list (max 3) of IP addresses of domain name servers for the engine VM
Engine VM DNS (leave it empty to skip) [10.1.64.254]: 10.1.64.254
​
...
​
[ INFO ] Stage: Setup validation
Please provide the hostname of this host on the management network [node2.mydomain]: node2.my
domain.ru

```

В процессе установки инсталлятор задаст вопрос, какой тип хранилища использовать (в данном случае NFS), а так же попросит указать расположение хранилища и размер диска управляющей машины (указать 60 ГБ минимум для работы механизма минорных обновлений):

<pre><code><strong>Please specify the storage you would like to use (glusterfs, iscsi, fc, nfs)[nfs]: nfs
</strong>Please specify the nfs version you would like to use (auto, v3, v4, v4_0, v4_1, v4_2)[auto]:
Please specify the full shared storage connection path to use (example: host:/path): nfs.pvhostvm.ru:/data/nfs_4

...

Please specify the size of the VM disk in GiB: [51]: 60
</code></pre>

**Во время обновления необходимо указать новый Storage Domain. Скрипт развертывания переименует Storage Domain и сохранит данные**

13\) После того, как развертывание Hosted HOSTVM Manager будет закончено, портал администрирования будет доступен по прежнему адресу

14\) Зайти на портал администрирования, проверить что кластер доступен и работает

Если инсталляция происходит на стенде, где нет настроенного DNS, а используется файл hosts, необходимо зайти в консоль ВМ Hosted HOSTVM Manager и настроить файл /etc/hosts, чтобы хосты в кластере стали доступны

\
