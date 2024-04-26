# Руководство по установке и настройке

Компоненты HOSTVM VDI поставляются в виде готовых виртуальных машин. Вы можете скачать их из каталога загрузок, доступного в [личном кабинете](https://lk.pvhostvm.ru).

Ознакомьтесь с [системными требованиями](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/requirements) для виртуальных машин HOSTVM VDI.

В каталоге загрузок образы виртуальных машин расположены в директории `HOSTVM-VDI/`, для загрузки доступны следующие версии:

`HOSTVM-VDI/beta` - версия ПО, находящаяся в режиме бета-тестирования;

`HOSTVM-VDI/stable` - текущий стабильный релиз(-ы);

`HOSTVM-VDI/legacy` - предыдущий стабильный релиз(-ы).

### Обязательные компоненты <a href="#mandatory" id="mandatory"></a>

**HOSTVM VDI Брокер**: брокер подключений, требуется во всех вариантах развертывания.

Предоставляет функционал портала пользователя и портала администратора системы, обеспечивает жизненный цикл виртуальных рабочих столов и приложений, взаимодействие с гипервизорами и другими сервис-провайдерами.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install).

Для установки **HOSTVM VDI Брокер** в виде пакета для систем РЕД ОС версии 7.3.3 и выше воспользуйтесь данной [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install/paketnaya-ustanovka-brokera-dlya-red-os)

Для установки  **HOSTVM VDI Брокер** в виде пакета для систем ALT Linux версии 10.02 и выше воспользуйтесь данной [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install/paketnaya-ustanovka-brokera-dlya-alt-linux)

### Дополнительные компоненты <a href="#optional" id="optional"></a>

**HOSTVM VDI Шлюз**: шлюз для предоставления защищенных подключений и HTML5 доступа к сервисам.

Используйте данный компонент, если вам требуется обеспечить защищенный доступ пользователей к сервисам VDI из WAN, и/или возможность подключения через HTML5.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy).

Для установки **HOSTVM VDI Шлюз** в виде пакета для РЕД ОС версии 7.3.3 и выше воспользуйтесь следующей [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy/paketnaya-ustanovka-tunnelera-dlya-red-os)

Для установки **HOSTVM VDI Шлюз** в виде пакета для ALT Linux версии 10.02 и выше воспользуйтесь следующей [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy/paketnaya-ustanovka-tunnelera-dlya-alt-linux)

**HOSTVM VDI Сервер БД**: сервер базы данных брокера подключений.

Используйте данный компонент, если вам требуется выделенный сервер БД брокера вместо встроенного в составе брокера, например как часть отказоустойчивой конфигурации, в случае отсутствия собственного сервера БД в инфраструктуре.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/vdi-db).

**HOSTVM VDI Балансировщик** - обеспечивает балансировку подключений к брокерам и шлюзам HOSTVM VDI при развертывании отказоустойчивой конфигурации.

Для импорта и настройки воспользуйтесь [инструкцией](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/haproxy).

### Дальнейшая настройка

Для дальнейшей настройки системы обратитесь к [руководству администратора](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-admin-guide).

### Перечень изменений

С перечнем изменений и исправлений вы можете ознакомиться в [статье](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/changelog).\
\
