# Подготовка NFS share

После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root.

Пароль root по умолчанию для новой версии hostvm node: HostvmNode.

Перейдите на вкладку Terminal

<figure><img src="https://lh4.googleusercontent.com/BMl4fJC6zMi1gz859ER0XyYLohOq94CKjkb91LkcdoFY8C2DxJQ2tNY-HLcjScrLRiecL8bOoV0xaVi6lOloOp9EPbLbOEYSt4nywluPJ0PdYRgQaNokwCxTWk-ux1gfzvzzKATGH4vIU7ckGBA1kvA" alt=""><figcaption></figcaption></figure>

Создайте папку `/data/nfs/iso-store`

```
root@testname1 ~]# mkdir -p /data/nfs/iso-store
```

Создайте пользователей, необходимых для работы с хранилищем.

```
[root@testname1 ~]# groupadd kvm -g 36
[root@testname1 ~]# useradd vdsm -u 36 -g 36
```

Если nfs-хранилище будет настроено на хосте виртуализации, то при создании появится сообщение, что пользователь и группа уже существуют

Настройте права доступа на созданную папку

```
[root@testname1 ~]# chown -R 36:36 /data/nfs/iso-store
[root@testname1 ~]# chmod 0755 /data/nfs/iso-store
```

Установим необходимые пакеты

```
[root@testname1 ~]# yum install nfs-utils -y
```

Добавим необходимые службы в автозагрузку и включим

```
[root@testname1 ~]# systemctl enable rpcbind nfs-server
[root@testname1 ~]# systemctl start rpcbind nfs-server
```

Настроим файл конфигурации nfs-сервера. Откройте файл /etc/exports для редактирования:

```
[root@testname1 ~]# nano /etc/exports
```

Введите следующий текст (вместо 10.1.99.0/24 введите вашу IP-подсеть):

```
/data/nfs/iso-store 10.1.99.0/24(rw)
```

Важно!: необходимо соблюдать формат записи, лишних пробелов быть не должно. В данной записи:

`/data/nfs/iso-store` - путь к папке, которая будет nfs-хранилищем;

`10.1.99.0/24` – IP-подсеть, которой разрешён доступ к nfs-хранилищу;

`(rw)` - набор опций для nfs-хранилища.

Сохраняем файл

Применяем новую конфигурацию командой:

```
[root@testname1 ~]# exportfs -r
```

Убедимся, что ресурсы опубликованы

```
[root@testname1 ~]# exportfs
# /data/nfs/iso-store 10.1.99.0/24
```

Если NFS-сервер сконфигурирован не на хосте виртуализации, необходимо добавить правила для файрволла на хосте. На NFS-сервере необходимо также добавить эти правила (в зависимости от ОС, команды могут отличаться), пример для firewalld:

```
[root@testname1 ~]# firewall-cmd --permanent --add-service=nfs
[root@testname1 ~]# firewall-cmd --permanent --add-service=mountd
[root@testname1 ~]# firewall-cmd --permanent --add-service=rpc-bind
[root@testname1 ~]# firewall-cmd --reload
```
