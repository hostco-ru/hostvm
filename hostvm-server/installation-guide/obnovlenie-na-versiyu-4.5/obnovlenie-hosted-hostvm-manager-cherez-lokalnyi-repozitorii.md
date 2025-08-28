# Обновление Hosted HOSTVM Manager через локальный репозиторий

{% hint style="warning" %}
Данная статья описывает процесс обновления управляющей машины через локальный репозиторий с последующим обновлением хостов.
{% endhint %}

### Подготовка к обновлению

{% hint style="info" %}
Во время обновления запущенные ВМ могут продолжать работу \
**Портал администрирования во время обновления будет недоступен**
{% endhint %}

1. Подключитесь к хосту, на котором расположена управляющая машина и переведите кластер в режим обслуживания:

```
hosted-engine --set-maintenance --mode=global
```

2. Убедитесь, что кластер перешел в режим обслуживания:

```
hosted-engine --vm-status
```

3. Подключитесь по SSH к ВМ Hosted HOSTVM Manager и остановите сервис:

```
systemctl stop ovirt-engine
```

4. Сделайте резервную копию конфигурации управляющей машины:

```
engine-backup --scope=all --mode=backup --file=backup.bck --log=backuplog.log
```

5. Сохраните созданный backup.bck, например, на свой ПК.

### Установка обновления

1. Скачайте из Личного Кабинета образ hostvm-update-local-repo и загрузите его на управляющую машину (например, с помощью WinSCP) в директорию:

```
/var/local/
```

2. Подключитесь к консоли управляющей машины и создайте директорию для локального репозитория:

```
mkdir -p /var/local/local-repo
```

3. Смонтируйте образ:

```
sudo  mount /var/local/hostvm-update-local-repo-*.iso /var/local/local-repo/
```

4. Выполните команду для обновления:

```
sh /var/local/local-repo/update*.sh | tee -a /root/hostvm-manager-update.log
```

После чего будет запущен процесс обновления.

### Пример ответов на вопросы инсталлятора

{% hint style="warning" %}
**Важно**

**На данный вопрос инсталятора необходимо ответить "No":**

<pre><code><strong>Do you want to abort Setup? (Yes, No) [Yes]: No
</strong></code></pre>
{% endhint %}

```
Replying "No" will abort Setup. You can pass the option "--offline" to prevent installing or updating packages.
Do you wish to update them now? (Yes, No) [Yes]: Yes
Do you want to abort Setup? (Yes, No) [Yes]: No
Do you want Setup to configure the firewall? (Yes, No) [Yes]: Yes
Would you like to backup the existing database before upgrading it? (Yes, No) [Yes]: Yes
Perform full vacuum on the oVirt engine history database ovirt_engine_history@localhost? (Yes, No) [No]: No
Apache httpd SSL was already configured in the past, but some needed changes are missing there. Configure again? (Yes, No) [Yes]: Yes  

  --== CONFIGURATION PREVIEW ==--
         
          Default SAN wipe after delete           : False
          Host FQDN                               : manager455
          Firewall manager                        : firewalld
          Update Firewall                         : True
          Upgrade packages                        : True
          Require packages rollback               : False
          Set up Cinderlib integration            : False
          Engine database host                    : localhost
          Engine database port                    : 5432
          Engine database secured connection      : False
          Engine database host name validation    : False
          Engine database name                    : engine
          Engine database user name               : engine
          Engine installation                     : True
          PKI organization                        : Test
          Set up ovirt-provider-ovn               : True
          DWH installation                        : True
          DWH database host                       : localhost
          DWH database port                       : 5432
          DWH database secured connection         : False
          DWH database host name validation       : False
          DWH database name                       : ovirt_engine_history
          DWH database user name                  : ovirt_engine_history
          Backup DWH database                     : True
          Grafana integration                     : True
          Grafana database user name              :
          ovirt_engine_history_grafana
          Configure VMConsole Proxy               : True
          Configure WebSocket Proxy               : True
         
          Please confirm installation settings (OK, Cancel) [OK]: OK
```

### После обновления

1. Убедитесь, что веб-портал доступен.
2. Поочередно обновите хосты в соответствии с [инструкцией](obnovlenie-hostvm-node.md).
3. Образ локального репозитория может быть размонтирован и удален:

```
umount /var/local/local-repo
rm -rf /var/local/hostvm-update-local-repo-*.iso
```

4. По умолчанию внешние репозитории на время обновления будут отключены, чтобы включить их, установите `enabled=1`  в следующем файле:

```
/etc/yum.repos.d/repo.pvhostvm.ru.repo
```
