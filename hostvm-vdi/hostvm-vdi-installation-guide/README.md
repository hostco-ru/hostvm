# Руководство по установке и настройке

Компоненты HOSTVM VDI поставляются в виде:

* готовых виртуальных машин, для импорта в систему виртуализации;
* deb и rpm пакетов, для установки на поддерживаемые операционные системы (Debian, РЕД ОС, ALT Linux).

Вы можете скачать их из каталога загрузок, доступного в [личном кабинете](https://lk.pvhostvm.ru).

Ознакомьтесь с [системными требованиями](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/requirements) для виртуальных машин HOSTVM VDI.

В каталоге загрузок пакеты и образы виртуальных машин расположены в директории `HOSTVM-VDI/`, для загрузки доступны следующие версии:

`HOSTVM-VDI/beta` - версия ПО, находящаяся в режиме бета-тестирования;

`HOSTVM-VDI/stable` - текущий стабильный релиз(-ы);

`HOSTVM-VDI/legacy` - предыдущий стабильный релиз(-ы).

### Обязательные компоненты <a href="#mandatory" id="mandatory"></a>

#### HOSTVM VDI Брокер <a href="#broker" id="broker"></a>

Брокер подключений, требуется во всех вариантах развертывания.

Предоставляет функционал портала пользователя и портала администратора системы, обеспечивает жизненный цикл виртуальных рабочих столов и приложений, взаимодействие с гипервизорами и другими сервис-провайдерами.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install).

Для установки **HOSTVM VDI Брокер** в виде пакета для систем РЕД ОС версии 7.3.3 и выше воспользуйтесь данной [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install/paketnaya-ustanovka-brokera-dlya-red-os)

Для установки  **HOSTVM VDI Брокер** в виде пакета для систем ALT Linux версии 10.02 и выше воспользуйтесь данной [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install/paketnaya-ustanovka-brokera-dlya-alt-linux)

#### HOSTVM VDI Клиент <a href="#client" id="client"></a>

Клиент HOSTVM VDI устанавливается на устройства, с которых осуществляется доступ к опубликованным сервисам (виртуальным рабочим столам, терминальным сессиям и приложениям). Наличие клиента требуется для всех типов подключений, кроме HTML5 (для них нужен только браузер).

Дистрибутивы клиента HOSTVM VDI доступны для загрузки [через веб-интерфейс](client/) брокера HOSTVM VDI.

### Дополнительные компоненты <a href="#optional" id="optional"></a>

#### HOSTVM VDI Шлюз <a href="#tunneler" id="tunneler"></a>

Шлюз для предоставления защищенных подключений и HTML5 доступа к сервисам.

Используйте данный компонент, если вам требуется обеспечить защищенный доступ пользователей к сервисам VDI из WAN, и/или возможность подключения через HTML5.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy).

Для установки **HOSTVM VDI Шлюз** в виде пакета для РЕД ОС версии 7.3.3 и выше воспользуйтесь следующей [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy/paketnaya-ustanovka-tunnelera-dlya-red-os)

Для установки **HOSTVM VDI Шлюз** в виде пакета для ALT Linux версии 10.02 и выше воспользуйтесь следующей [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy/paketnaya-ustanovka-tunnelera-dlya-alt-linux)

#### HOSTVM VDI Сервер БД <a href="#dbserver" id="dbserver"></a>

Сервер базы данных брокера подключений.

Используйте данный компонент, если вам требуется выделенный сервер БД брокера вместо встроенного в составе брокера, например как часть отказоустойчивой конфигурации, в случае отсутствия собственного сервера БД в инфраструктуре.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/vdi-db).

#### HOSTVM VDI Балансировщик <a href="#load-balancer" id="load-balancer"></a>

Обеспечивает балансировку подключений к брокерам и шлюзам HOSTVM VDI при развертывании отказоустойчивой конфигурации.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/haproxy).

#### HOSTVM VDI Агент <a href="#actor" id="actor"></a>

Агент HOSTVM VDI устанавливается:

* в шаблоны операционных систем Windows или Linux (базовый (“золотой”) образ) для управления публикацией виртуальных рабочих столов;
* на терминальные серверы RDS и Static IP машины для управления сессиями пользователей.

Дистрибутивы агента HOSTVM VDI доступны для загрузки [в веб-интерфейсе брокера](actor/) HOSTVM VDI из-под учетной записи с правами администратора.

#### HOSTVM VDI Лаунчер <a href="#launcher" id="launcher"></a>

Приложение, позволяющее получать доступ к витрине сервисов без открытия web-портала в браузере.

Дистрибутивы расположены в каталоге загрузок в директории `HOSTVM-VDI/Misc/Launcher`.

[Руководство по установке и настройке](launcher.md)

[Руководство пользователя](../hostvm-vdi-user-manual/launcher.md)

### Высокая доступность

Для конфигурации системы в режиме высокой доступности обратитесь к разделу документации [Конфигурация высокой доступности](high-availability/).

### Дальнейшая настройка

Для дальнейшей настройки системы обратитесь к [руководству администратора](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-admin-guide).

### Перечень изменений

С перечнем изменений и исправлений вы можете ознакомиться в [статье](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/changelog).\
<br>
