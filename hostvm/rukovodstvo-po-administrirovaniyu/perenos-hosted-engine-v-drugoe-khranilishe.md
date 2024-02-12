# Перенос Hosted-Engine в другое хранилище

Для переноса Hosted-Engine необходимо выполнить резервное копирование существующей установки, и новую установку на целевое хранилище с восстановлением данных из резервной копии. Ключевые действия при переносе Hosted-Engine:&#x20;

·         Резервное копирование Hosted-Engine

·         Установка Hosted-Engine на новое хранилище и восстановление данных из резервной копии

·         Обновление конфигурации хостов для работы с новой ВМ Hosted-Engine

·         Удаление старого домена хранения Hosted-Engine

**До начала переноса необходимо выполнить следующие условия:**

·         Переместить все диски виртуальных машин с хранилища, на котором расположена ВМ Hosted- Engine (кроме дисков этой машины Hosted-Engine). Выполнение процедуры переноса делает недоступным исходное хранилище, поэтому убедитесь, что оно не содержит дисков ВМ, иначе они будут потеряны.

·         Обеспечить разрешение имен Hosted-Engine и хоста, с которого будет выполняться установка. Новый Hosted-Engine должен иметь такой же FQDN, как и исходный.

·         Обеспечить наличие как минимум одного или нескольких хостов без роли selfhosted. На него необходимо переключить роль SPM через портал администрирования.

·         Подготовить новое хранилище для развертывания Hosted-Engine, удовлетворяющее системным требованиям

### Резервное копирование исходной ВМ Hosted Engine

Подключитесь по SSH на хост, на котором запущена ВМ с Hosted Engine. Выполните:

```
# hosted-engine --set-maintenance --mode=global
```

Убедитесь, что кластер перешел в режим global maintenance:

```
# hosted-engine --vm-status
```

Подключитесь по SSH к ВМ Hosted Engine. Остановите службу:

```
# systemctl stop ovirt-engine
# systemctl disable ovirt-engine
```

Сделайте резервную копию. Команда в примере создает файл архива с резервной копией в /root/hvm-backup.tgz и файл журнала в /root/backup.log :

```
# engine-backup --scope=all --mode=backup --file=/root/hvm-backup.tgz --log=/root/backup.log
```

Файл архива (в примере /root/hvm-backup.tgz ) содержит резервную копию конфигурационных файлов и баз данных менеджера виртуализации. Этот файл должен быть скопирован с машины Hosted Engine на внешнее хранилище, для дальнейшего использования, например:

```
# scp -p /root/hvm-backup.tgz /root/backup.log storage.example.com:/backup/
```

На хосте, на котором запущена ВМ с Hosted Engine, выполните команду для ее выключения:

```
# hosted-engine --vm-shutdown
```

Убедитесь что ВМ в статусе down на всех хостах, проверив вывод команды:

```
# hosted-engine --vm-status
```

### Новая установка и восстановление из резервной копии

1. Загрузите файл резервной копии на хост, с которого будет выполнятся новая установка Hosted Engine.
2. Подключитесь к хосту, выполните вход в ОС.
3. Выполните сброс хоста

```
# ansible-playbook /etc/ansible/clean-node.yml
```

4. Используйте оконный менеджер screen (или tmux), чтобы не потерять сессию в случае обрыва соединения:

<pre><code># yum install screen
<strong># screen
</strong></code></pre>

При обрыве соединения для восстановления сессии используйте

```
screen -d -r
```

5. Запустите новую установку Hosted Engine, указав местоположение файла резервной копии:

```
# hosted-engine --deploy --restore-from-file=backup/file_name
```

Ответьте на вопросы инсталлятора. Если не указано иное, используйте опции по умолчанию (нажатием Enter). Ниже указан пример ответов на вопросы инсталлятора.&#x20;

6. Выберите Yes для начала развертывания.

```
        Continuing will configure this host for serving as hypervisor and will 
create a local VM with a running engine. 
        The provided engine backup file will be restored there, 
        it's strongly recommended to run this tool on an host that wasn't part of 
the environment going to be restored. 
        If a reference to this host is already contained in the backup file, it will 
be filtered out at restore time. 
        The locally running engine will be used to configure a new storage domain 
and create a VM there. 
        At the end the disk of the local VM will be moved to the shared storage. 
        The old hosted-engine storage domain will be renamed, after checking that 
everything is correctly working you can manually remove it. 
        Other hosted-engine hosts have to be reinstalled from the engine to update \
their hosted-engine configuration. 
        Are you sure you want to continue? (Yes, No)[Yes]: Yes 
```



7. Укажите адрес шлюза и имя интерфейса, которое будет использовано как виртуальный коммутатор.&#x20;

```
[ INFO ] Bridge ovirtmgmt already created 
         Please indicate the gateway IP address [10.1.158.1]: 10.1.158.1
```

<pre><code>[ INFO ] TASK [ovirt.hosted_engine_setup : Validate selected bridge interface if 
management bridge does not exist] 
[ INFO ] skipping: [localhost] 
<strong>         Please indicate a nic to set ovirtmgmt bridge on: (enp8s0f0) [enp8s0f0]: 
</strong>enp8s0f0 
         Please specify which way the network connectivity should be checked (ping,
dns, tcp, none) [dns]: dns
</code></pre>

8. Введите пароль пользователя root для ВМ HOSTVM Manager.&#x20;

```
Enter root password that will be used for the engine appliance: 
Confirm appliance root password: 
```

9. Задайте сетевые настройки для ВМ. В случае Static задайте IP адрес машины. Адрес должен принадлежать к той же подсети, что и адрес хоста.&#x20;

<pre><code>--== VM CONFIGURATION ==-- 

How should the engine VM network be configured (DHCP, Static)[DHCP]? Static 
How should the engine VM network be configured (DHCP, Static)[DHCP]? Static 
<strong>         Please enter the IP address to be used for the engine VM []: 10.1.158.141 
</strong>         Please provide a comma-separated list (max 3) of IP addresses of domain name 
servers for the engine VM 
<strong>         Engine VM DNS (leave it empty to skip) [10.1.64.254]: 10.1.64.254 
</strong></code></pre>

10. Укажите, следует ли добавить записи для Hosted Engine и хоста в файл /etc/hosts виртуальной машины. Вы должны убедиться, что эти имена будут разрешаться в IP адреса.&#x20;

```
Add lines for the appliance itself and for this host to /etc/hosts on the engine VM?
         Note: ensuring that this host could resolve the engine VM hostname is still
up to you 
         (Yes, No)[No] 
```

11. Введите пароль пользователя admin@internal для доступа к порталу администрирования.&#x20;

```
Enter engine admin password: 
Confirm engine admin password: 
```

12. Скрипт начнет создание виртуальной машины. Это может занять некоторое время.&#x20;
13. Выберите тип хранилища для ВМ.&#x20;

```
Please specify the storage you would like to use (glusterfs, iscsi, fc, nfs)[nfs]: 
```

14. Укажите путь к новому хранилищу ВМ. Не используйте точку монтирования старого домена хранения менеджера виртуализации.

```
Please specify the full shared storage connection path to use (example: host:/path):  
```

15. Задайте размер диска ВМ (указать 60 ГБ минимум для работы механизма минорных обновлений).

```
Please specify the size of the VM disk in GiB: [51]: 60 
```

16. Скрипт продолжит выполнение до завершения развертывания. Успешное завершение будет обозначено сообщением:

```
[ INFO ] Hosted Engine successfully deployed 
```

17. После того как развертывание Hosted Engine будет закончено, портал администрирования будет доступен по прежнему адресу.

### Обновление конфигурации хостов

После завершения развертывания и восстановления работы менеджера виртуализации, необходимо через портал администрирования выполнить переустановку хостов с ролью self-hosted engine для обновления их конфигурации. Для стандартных хостов без этой роли дополнительные действия не требуются. Выполните следующую процедуру для каждого из хостов с ролью self-hosted engine:

1. Перейдите в раздел **Compute > Hosts** и выделите нужный хост.
2. Переведите хост в режим обслуживания, нажав кнопку **Management > Maintenance** и затем **OK**.
3. После завершения перевода, нажмите кнопку **Installation > Reinstall**.
4. В открывшемся окне перейдите на вкладку **Hosted Engine** и выберите действие **DEPLOY** из выпадающего списка.
5. Нажмите **OK** для переустановки хоста. После завершения переустановки и перехода хоста в статус **Up**, вы можете мигрировать виртуальные машины обратно на этот хост. После переустановки хостов с ролью self-hosted engine, вы можете проверить статус системы, выполнив на одном из этих хостов команду:

```
#  hosted-engine --vm-status
```

### Удаление старого домена хранения&#x20;

В процессе восстановления старый домен хранения менеджера виртуализации был переименован, но не удален из системы. После того, как вы убедитесь, что восстановление прошло успешно и система работает корректно, этот домен можно удалить.
