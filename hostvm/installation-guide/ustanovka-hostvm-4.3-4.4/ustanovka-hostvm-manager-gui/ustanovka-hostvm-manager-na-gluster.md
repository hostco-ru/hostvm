# Установка HOSTVM Manager на Gluster

После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root.

Пароль root по умолчанию для новой версии hostvm node: HostvmNode.

Пример адреса: https://192.168.0.5:9090\
\
На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины hosted-engine, которая будет установлена.\
\
Если на DNS-сервере отсутствуют записи, то их можно добавить вручную на ноде HOSTVM: [deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns.md](../../deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns.md "mention")

Перейдите на вкладку Терминал и поочередно введите команды:

```
root@testname1 ~]# ssh-keygen
root@testname1 ~]# ssh-copy-id root@flexnode1
```

\*root - имя пользователя

\*flexnode1 - FQDN хоста, на котором будет хранилище

![](<../../../../.gitbook/assets/image (40).png>)

Далее нужно создать раздел, для этого заходим в Storage -> Create partition

![](<../../../../.gitbook/assets/image (3) (1) (1) (1) (1) (1) (1).png>)

Задаем параметры, как на скриншоте ниже, определяем размер раздела (не меньше 90 GB). Type раздела выбираем No filesystem.

![](<../../../../.gitbook/assets/image (2) (2) (1).png>)

Запоминаем, какой раздел был создан, в нашем случае /dev/sda3

![](<../../../../.gitbook/assets/image (52).png>)

Далее заходим в Virtualization -> Hosted Engine -> Hyperconverged

![](<../../../../.gitbook/assets/image (35).png>)

Выбираем Run Gluster Wizard For Single Node для установки хранилища на одной ноде

![](<../../../../.gitbook/assets/image (42) (1).png>)

Вводим имя хоста, для которого генерировали ключи, жмем next

![](<../../../../.gitbook/assets/image (32).png>)

Оставляем как есть, жмем next

![](<../../../../.gitbook/assets/image (44) (1) (1).png>)

Удаляем data(1) и vmstore(2), жмем next

<figure><img src="../../../../.gitbook/assets/1.png" alt=""><figcaption></figcaption></figure>

·         Выбираем нужный тип RAID

·         В Device Name выбираем раздел, который был создан ранее (/dev/sda3)

·         Задаем размер LV Size в соответствии с системными требованиями, жмем next.

<figure><img src="../../../../.gitbook/assets/3.png" alt=""><figcaption></figcaption></figure>

Нажимаем deploy

![](<../../../../.gitbook/assets/image (38) (1).png>)

На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины hosted-engine, которая будет установлена.

Заполните форму. Виртуальная машина создается со статическим файлом с использованием файла hosts.

![](<../../../../.gitbook/assets/image (31).png>)

Заполните следующую форму. Укажите пароль от веб-интерфейса будущей виртуальной машины. Заполните настройки отправки уведомлений e-mail при необходимости.

![](<../../../../.gitbook/assets/image (39) (1).png>)

Проверяем параметры, нажимаем Prepare VM

![](<../../../../.gitbook/assets/image (43) (1).png>)

Выбираем Storage Type = Gluster

Storage Connection = flexnode1:/engine

\*flexnode1 – имя ноды

\*engine – местонахождение хранилища

![](<../../../../.gitbook/assets/image (46) (1).png>)

## Если что-то пошло не так

1. Проверьте корректность вводимых данных в сценарии развёртывания. При обнаружении ошибок выполните команду `/usr/sbin/ovirt-hosted-engine-cleanup`, очистите хранилище и начните сначала.
2. Если после завершения установки не открывается страница в браузере с адресом `https://engine.mydomain.ru`, то

* проверьте, что Ip для управляющей машины, указанный в таблице в начале установки, отвечает на команду `ping`;
* проверьте, что имя engine.mydomain.ru разрешается вашим dns-сервером.

Если устранить проблему не удалось, обратитесь в [техническую поддержку](https://lk.pvhostvm.ru/) используя [инструкцию](https://lk.pvhostvm.ru/) К обращению приложите логи установки, которые находятся в директории `/var/log/ovirt-hosted-engine-setup`.
