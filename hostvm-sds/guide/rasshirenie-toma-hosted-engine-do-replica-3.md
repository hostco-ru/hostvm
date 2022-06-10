# Расширение тома Hosted-engine до Replica 3

Для расширения тома hosted-engine на GlusterFS дополнительными бриками до Replica 3, выполните следующие шаги:

### Создание дополнительных записей имен серверов

На каждый из дополнительных серверов будущего кластера и на ВМ engine добавить в файлы `/etc/hosts` записи следующего вида:

```
192.168.120.10    glusternode1    glusternode1.localdomain
192.168.120.11    glusternode2    glusternode2.localdomain
192.168.120.12    glusternode3    glusternode3.localdomain
```

* glusternode1-3 имя нод SDS (второе имя для серверов и их ip-адреса на выделенных под SDS интерфейсах).
* Опционально можно прописать все имена на DNS-сервере.

### Создание бэкапа Engine:

Подключитесь к виртуальной машине с Engine и выполните команду:

```
engine-backup --scope=all --mode=backup --file=backup_03.06.22 --log=backup_03.06.22_log
```

* Путь по умолчанию к создаваемому бэкапу - /var/lib/ovirt-engine-backup/ovirt-engine-backup-YmdHMS.backup

### Создание бриков на хостах:

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

Монтируем том:

```
mount /dev/mapper/datavg-datastore_lv /data/gluster/datastore
```

На каждом из дополнительных серверов создать директории под брики и запустить службы sds:

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

Добавим хосты в зону видимости друг друга, на хосте с hosted-engine выполняем команду (повторяем для всех дополнительных хостов):

```
gluster peer probe glusternode2
```

Проверяем статус присоединенного сервера командой:

```
gluster peer status
```

Данная команда может быть выполнена также и на добавляемом хосте, соединение будет установлено в двустороннем порядке.

### Инициализация хранилища Replica 3:

На хосте с Engine добавляем дополнительный брик к тому hosted-engine командой:

```
gluster volume add-brick hosted-engine replica 2 имя_сервера_2:/data/gluster/datastore/brick1
```

Аналогично добавляем еще один брик:

```
gluster volume add-brick hosted-engine replica 3 имя_сервера_3:/data/gluster/datastore/brick1
```

Либо одной длинной командой:

```
gluster volume add-brick hosted-engine replica 3 имя_сервера_2:/data/gluster/datastore/brick1 имя_сервера_3:/data/gluster/datastore/brick1
```

Проверяем статус тома:

```
gluster volume status
gluster volume info
```

### Добавление backup-volfile-servers в mount point

Добавим опцию backup-volfile-servers в mount point, для этого переключаемcя в режим в global maintenance:

```
hosted-engine --set-maintenance --mode=global
```

Выключаем виртуальную машину с HE:

```
hosted-engine --vm-shutdown
```

Добавляем строку в файл /etc/ovirt-hosted-engine/hosted-engine.conf:

```
mnt_options = backup-volfile-servers=glusternode2:glusternode3 [имена серверов]
```

Дополнительно выполняем команды:

```
hosted-engine --set-shared-config mnt_options backup-volfile-servers=virt2 --type=he_local
```

```
hosted-engine --set-shared-config mnt_options backup-volfile-servers=virt2 --type=he_shared
```

где:

\[he\_local] – задает значение в локальном экземпляре файла /etc/ovirt-hosted-engine/hosted-engine.conf на локальном хосте, чтобы только этот хост использовал новые значения. Чтобы включить новое значение, перезапустите службы ovirt-ha-agent и ovirt-ha-broker.

\[he\_shared] - задает значение в файле /etc/ovirt-hosted-engine/hosted-engine.conf в shared storage, чтобы все хосты, развернутые после изменения конфигурации, использовали эти значения. Чтобы включить новое значение на хосте, повторно разверните этот хост.

Проверим что параметр добавился корректно (на уровне веб-интерфейса данный параметр может не отобразится):

```
hosted-engine --get-shared-config mnt_options --type=he_local
```

```
hosted-engine --get-shared-config mnt_options --type=he_shared
```

Если эта опция применена, то при сбое первого сервера volfile серверы, указанные в опции backup-volfile-servers, используются в качестве серверов volfile для монтирования клиента до тех пор, пока монтирование не завершится успешно.

Перезагружаем хост с hosted-engine командой reboot;

После перезапуска убедимся, что виртуальная машина стартовала (ожидание может быть длительным):

```
hosted-engine --vm-status
```

```
hosted-engine --vm-start
```

Выводим HE из режима global maintenance:

```
hosted-engine --set-maintenance --mode=none
```

### Добавление хостов

Включаем Enable Gluster Service в дефолтном кластере дефолтного датацентра:

![](../../.gitbook/assets/gluster\_service.png)

Даём хостам беспарольный доступ по SSH (выполняется на каждом хосте):

```
ssh-keygen
```

```
ssh-copy-id root@glusternode2
```

Добавляем хост, для этого в Engine переходим в раздел Compute -> Hosts, нажимаем New и указываем данные добавляемого хоста:

* Name - имя хоста
* Hostname - FQDN хоста
* В разделе Authentication выбираем SSH Public Key и копируем содержимое поля правее в файл /root/.ssh/authorized\_keys добавляемого хоста.
