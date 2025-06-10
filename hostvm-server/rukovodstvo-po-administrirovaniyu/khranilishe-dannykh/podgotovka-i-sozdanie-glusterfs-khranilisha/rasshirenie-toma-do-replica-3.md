# Расширение тома до replica 3

В данном руководстве описывается процесс расширения тома GlusterFS, состоящего из одного брика, до replica 3.

### Создание дополнительных записей имен серверов

На каждый из дополнительных серверов кластера и на ВМ HOSTVM Manager добавить в файлы `/etc/hosts` записи следующего вида:

```
192.168.120.10    glusternode1    glusternode1.localdomain
192.168.120.11    glusternode2    glusternode2.localdomain
192.168.120.12    glusternode3    glusternode3.localdomain
```

* glusternode1-3 имя нод GlusterFS (второе имя для серверов и их IP-адреса на выделенных под Gluster интерфейсах).
* Опционально можно прописать все имена на DNS-сервере.

### Создание бриков на хостах

#### Разметка дискового пространства:

Допустим, что под новый брик мы хотим выделить 120 ГБ.

Для размещения используем рейд-массив дисков, представленный в виде устройства `/dev/sdX`.

На каждом из серверов создадим logical volume:

```
pvcreate /dev/sdxY
vgcreate datavg /dev/sdxY
lvcreate -L 120G -n datastore_lv datavg
mkfs.xfs /dev/mapper/datavg-datastore_lv
mkdir -p /data/gluster/datastore 
```

* datavg – имя новой вольюм группы;
* datastore\_lv – имя вольюма в составе вольюм группы datavg.
* /data/gluster/datastore - точка монтирования

После того, как созданы том и файловая система, следующий шаг – создать запись в `/etc/fstab`:

```
/dev/mapper/datavg-datastore_lv /data/gluster/datastore xfs defaults 0 0
```

И примонтировать том:

```
mount /dev/mapper/datavg-datastore_lv /data/gluster/datastore
```

На каждом из серверов создать директории под брики и запустить службу glusterd:

```
mkdir /data/gluster/datastore/brick1 
systemctl start glusterd && systemctl enable glusterd
```

Открываем требуемые порты на каждом сервере:

```
firewall-cmd --zone=public --add-port=24007-24008/tcp --permanent 
firewall-cmd --zone=public --add-port=24009/tcp --permanent 
firewall-cmd --zone=public --add-port=111/tcp --permanent 
firewall-cmd --zone=public --add-port=139/tcp --permanent
firewall-cmd --zone=public --add-port=445/tcp --permanent 
firewall-cmd --zone=public --add-port=965/tcp --permanent 
firewall-cmd --zone=public --add-port=2049/tcp --permanent 
firewall-cmd --zone=public --add-port=38465-38469/tcp --permanent 
firewall-cmd --zone=public --add-port=631/tcp --permanent 
firewall-cmd --zone=public --add-port=111/udp --permanent 
firewall-cmd --zone=public --add-port=963/udp --permanent 
firewall-cmd --zone=public --add-port=49152-49251/tcp --permanent 
firewall-cmd --zone=public --add-service=nfs --permanent 
firewall-cmd --zone=public --add-service=samba --permanent 
firewall-cmd --zone=public --add-service=samba-client --permanent 
firewall-cmd –reload
```

### Инициализация хранилища replica 3

Дальнейшая настройка осуществляется с одного из серверов кластера т.е. _приведённые ниже команды нужно запускать только с одного из серверов_.

В примере команды выполняются с glusternode1.

Добавим ноды GlusterFS в зону видимости друг друга:

```
root@glusternode1:~# gluster peer probe glusternode2
root@glusternode1:~# gluster peer probe glusternode3
```

Добавляем дополнительный брик к тому datastore командой:

```
gluster volume add-brick datastore replica 2 имя_сервера_2:/data/gluster/datastore/brick1
```

Аналогично добавляем еще один брик:

```
gluster volume add-brick datastore replica 3 имя_сервера_3:/data/gluster/datastore/brick1
```

Либо одной длинной командой:

```
gluster volume add-brick datastore replica 3 имя_сервера_2:/data/gluster/datastore/brick1 имя_сервера_3:/data/gluster/datastore/brick1
```

Проверяем статус тома:

```
gluster volume status
gluster volume info
```

### Дополнительные настройки при расширении хранилища, содержащего HOSTVM Manager

Перед расширением тома, содержащего управляющую машину, переведите кластер в global maintenance:

```
hosted-engine --set-maintenance --mode=global
```

Подключитесь к управляющей машине и остановите сервис:

```
systemctl stop ovirt-engine
```

Создайте резервную копию конфигурации управляющей машины:

```
engine-backup --scope=all --mode=backup --file=backup.bck --log=backuplog.log
```

Сохраните файл резервной копии на хост/ПК.

Выключите управляющую машину:

```
hosted-engine --vm-shutdown
```

На хосте добавьте следующую строку в файл /etc/ovirt-hosted-engine/hosted-engine.conf:

```
mnt_options = backup-volfile-servers=glusternode2:glusternode3 #имена серверов
```

Дополнительно выполните команды:

```
hosted-engine --set-shared-config mnt_options backup-volfile-servers=glusternode2:glusternode3 --type=he_local
```

```
hosted-engine --set-shared-config mnt_options backup-volfile-servers=glusternode2:glusternode3 --type=he_shared
```

где:

\[he\_local] – задает значение в локальном экземпляре файла /etc/ovirt-hosted-engine/hosted-engine.conf на локальном хосте, чтобы только этот хост использовал новые значения. Чтобы включить новое значение, перезапустите службы ovirt-ha-agent и ovirt-ha-broker.

\[he\_shared] - задает значение в файле /etc/ovirt-hosted-engine/hosted-engine.conf в shared storage, чтобы все хосты, развернутые после изменения конфигурации, использовали эти значения. Чтобы включить новое значение на хосте, повторно разверните этот хост.

Убедитесь, что параметр добавился корректно:

```
hosted-engine --get-shared-config mnt_options --type=he_local
```

```
hosted-engine --get-shared-config mnt_options --type=he_shared
```

Если эта опция применена, то при сбое первого сервера volfile серверы, указанные в опции backup-volfile-servers, используются в качестве серверов volfile для монтирования клиента до тех пор, пока монтирование не завершится успешно.

Перезагрузите хост, содержащий управляющую машину.

Включите упраляющую машину:

```
hosted-engine --vm-start
```

Убедитесь, что виртуальная машина запустилась:

```
hosted-engine --vm-status
```

Выведите кластер из режима обслуживания:

```
hosted-engine --set-maintenance --mode=none
```
