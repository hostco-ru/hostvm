# Установка HOSTVM VDI Appliance

## Общие данные

Данный документ представляет собой руководство по развертыванию подготовленной OVA для предоставления сервиса VDI в систему управления виртуализацией HOSTVM.

## Инструкция по развертыванию версии 2.2

### Требования к конфигурации машины

Минимальные требования к ресурсам:

* процессор: 4 ядра;
* объем ОП: 4 ГБ;
* жесткий диск: 20 ГБ;
* сетевой адаптер: 1 Гбит;

### Развертывание OVA

* Скачать файл HOSTVM-VDI/vdi-appliance.ova из каталога  [https://reestr.hostco.ru/downloads](https://reestr.hostco.ru/downloads)
* Загрузить данный файл на один из хостов подключенных к HOSTVM с помощью scp или других средств
* Авторизоваться в веб-интерфейсе виртуализации, перейти в пункт **Compute - Virtual Machines**
* Нажать на иконку из трех вертикальных точек в правом верхнем углу интерфейса и выбрать пункт **Import**
* Выбрать **Source** "**Virtual Appliance\(OVA\)**" и указать абсолютный путь директории с файлом на хосте, нажать кнопку Load для поиска образа
* Выбрать нужный образ, нажать на стрелочку для помещения его в список ВМ для импорта и нажать кнпоку **Next**
* В появившемся окне, указать нужное место хранения ВМ, кластер и прочее.
* Также нужно выбрать строчку с именем ВМ, перейти на вкладку **Network Interfaces** и указать нужную сеть для данной ВМ.
* После этого с помощью кнопки **OK** запустить импорт OVA и дождаться окончания импорта.
* Запустить полученную ВМ, подключиться к ней с помощью консоли.
* Авторизоваться в ВМ с логином/паролем _uds/engine_, переключиться на пользователя root \(_root/engine_\)
* Изменить IP адрес ВМ на нужные с помощью редактирования файла через vi \(или другие редакторы\): `/etc/network/interfaces` - необходимо задать IP адрес, маску сети, шлюз по умолчанию, адреса DNS серверов и домен для поиска.
* Выполнить перезагрузку или команды: `ifdown <network interface> && ip addr flush <network interface> && ifup <network interface>` для установки нового IP
* При необходимости, изменить имя ВМ.
* Добавить FQDN ВМ в файл `/etc/hosts` с указанием нового IP адреса.
* Авторизоваться в веб-интерфейсе управления UDS через браузер по ссылке `https://%IP` - логин/пароль для входа _admin/engine_
* Изменить пароль административной учетной записи перейдя по ссылке `https://%IP/adm/`, пункт меню Инструменты - Конфигурация.
* Далее, выполнить настройку согласно руководству администратора.

## Инструкция по развертыванию версии 3.0

### Требования к конфигурации машины

Минимальные требования к ресурсам:

* процессор: 4 ядра;
* объем ОП: 4 ГБ;
* жесткий диск: 15 ГБ;
* сетевой адаптер: 1 Гбит;

### Развертывание OVA

* Скачать файл HOSTVM-VDI/hostvm-vdi.ova.tar.bz2 из каталога загрузок HOSTVM.
* Загрузить данный файл на один из хостов виртуализации к HOSTVM с помощью scp или других средств.
*  Распаковать: `tar xjvf hostvm-vdi.ova.tar.bz2`
* Авторизоваться в веб-интерфейсе виртуализации, перейти в пункт **Compute - Virtual Machines.**
* Нажать на иконку из трех вертикальных точек в правом верхнем углу интерфейса и выбрать пункт **Import.**
* Выбрать **Source** "**Virtual Appliance\(OVA\)**" и указать абсолютный путь директории с файлом на хосте, нажать кнопку Load для поиска образа.
* Выбрать нужный образ, нажать на стрелочку для помещения его в список ВМ для импорта и нажать кнпоку **Next.**
* В появившемся окне, указать нужное место хранения ВМ, кластер и прочее.
* Также нужно выбрать строчку с именем ВМ, перейти на вкладку **Network Interfaces** и указать нужную сеть для данной ВМ.
* После этого с помощью кнопки **OK** запустить импорт OVA и дождаться окончания импорта.
* Запустить полученную ВМ, подключиться к ней с помощью консоли.
* Авторизоваться в ВМ с логином/паролем _root/engine_
* Изменить IP адрес ВМ на нужные с помощью редактирования файла через vi \(или другие редакторы\): `/etc/network/interfaces` - необходимо задать IP адрес, маску сети, шлюз по умолчанию, адреса DNS серверов и домен для поиска.
* Выполнить перезагрузку или команды: `ifdown <network interface> && ip addr flush <network interface> && ifup <network interface>` для установки нового IP.
* При необходимости, изменить имя ВМ.
* Добавить FQDN ВМ в файл `/etc/hosts` с указанием нового IP адреса.
* Авторизоваться в веб-интерфейсе управления UDS через браузер по ссылке `https://%IP` - логин/пароль для входа _root/udsmam0_
* Изменить пароль административной учетной записи перейдя по ссылке `https://%IP/uds/adm/`, пункт меню Инструменты - Конфигурация - Security, параметр rootPass.
* Далее, выполнить настройку согласно руководству администратора.

