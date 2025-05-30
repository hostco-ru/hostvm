# Подготовка NFS хранилища

Учетные записи и группы системных пользователей требуются хостам HOSTVM, чтобы менеджер виртуализации мог хранить данные в доменах хранения, представленных экспортированными директориями. Следующая процедура устанавливает разрешения для одного каталога. Вы должны повторить шаги chown и chmod для всех каталогов, которые собираетесь использовать в качестве доменов хранения в HOSTVM Node.

Предварительная подготовка – установите необходимые пакеты, добавьте службы в автозагрузку и запустите:

```
#  yum install nfs-utils -y
# systemctl enable nfs-server
# systemctl enable rpcbind
```

Процедура настройки:

1\.  Создайте пользователей, необходимых для работы с хранилищем. Если nfs-хранилище будет настроено на одном из хостов виртуализации, этот шаг можно пропустить, потому что пользователь и группа уже существуют:

```
# groupadd kvm -g 36
# useradd vdsm -u 36 -g kvm
```

2\.  Создайте директорию, где будет располагаться хранилище, настройте права доступа на нее:

```
# mkdir /storage
```

{% hint style="info" %}
Если к созданной директории необходимо примонтировать устройство, то выполните следующую команду перед выполнением дальнейших действий:

<pre><code><strong>mount /dev/&#x3C;имя устройства> /data/nfs/iso-storage
</strong></code></pre>
{% endhint %}

```
# chmod 0755 /storage
# chown 36:36 /storage/
```

3\.  Добавьте директорию storage в /etc/exports с соответствующими разрешениями:

```
# vi /etc/exports
# cat /etc/exports
/storage 10.46.11.0/24(rw)
```

4\.  Перезапустите сервисы:

```
# systemctl restart rpcbind
# systemctl restart nfs-server
```

5\.  Убедитесь, что ресурсы опубликованы:

```
# exportfs
/storage   10.46.11.0/24
```

В случае если изменения в /etc/exports были внесены после запуска сервисов можно использовать команду exportfs -ra для применения изменений. После выполнения всех вышеперечисленных этапов общий ресурс storage будет готов к использованию.

6. Если NFS-сервер сконфигурирован не на хосте виртуализации, необходимо добавить правила для файрволла на хосте. На NFS-сервере необходимо также добавить эти правила (в зависимости от ОС, команды могут отличаться), пример для firewalld:

```
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=mountd
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --reload
```
