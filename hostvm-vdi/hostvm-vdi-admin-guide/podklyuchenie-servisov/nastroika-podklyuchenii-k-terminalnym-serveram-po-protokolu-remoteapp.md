# Настройка подключений к терминальным серверам по протоколу RemoteApp

#### **Регистрация сервис-провайдера Static IP machines provider**

Для регистрации сервис-провайдера необходимо зайти в раздел «Services», нажать «New» и выбрать тип провайдера Static IP machines provider.

![](../../../.gitbook/assets/vdi\_rds\_1.jpg)

Указать произвольное имя и сохранить провайдера.

#### Настройка сервиса на основе провайдера Static IP machines provider

Для создания сервисов открыть созданный провайдер двойным щелчком. Выбрать New>Static single IP

![](../../../.gitbook/assets/vdi\_rds\_2.jpg)

Указать имя (произвольное) и IP адрес терминального сервера Windows, на котором опубликованы приложения.

#### **Настройка транспорта (протокола) для прямого подключения к сервисам**

Для создания транспорта перейдите в соответствующий раздел панели администрирования VDI, затем New>Direct>выберите транспорт RemoteApp из списка.

![](../../../.gitbook/assets/vdi\_rds\_3.jpg)

#### **Транспорт «RemoteApp»**

Транспорт «RemoteApp» позволяет пользователям получать доступ к виртуальным приложениям Windows. После создания необходимо задать параметры транспорта.

Вкладка Основные:

* имя (Name) – имя транспорта;
* приоритет (Priority) – приоритет, который будет иметь транспорт. Чем ниже значение параметра, тем выше транспорт будет отображаться в списке доступных транспортов для сервиса. Транспорт с самым низким приоритетом будет использоваться по умолчанию при нажатии на иконку сервиса;

Вкладка Параметры:

* имя приложения App Name – параметр Alias (псевдоним) свойств опубликованного приложения:

![](../../../.gitbook/assets/vdi\_rds\_4.jpg)

#### Настройка сервис-пула для подключения к терминальным серверам и приложениям

Для создания пула сервисов потребуются ранее настроенные компоненты: базовый сервис сервис-провайдера, менеджер ОС, транспорты подключения, и группа пользователей, которым планируется предоставить доступ к сервису.

Для создания нового пула перейдите в раздел Pools>Service pools и нажмите «New».

![](../../../.gitbook/assets/vdi\_rds\_5.jpg)

Основные параметры:

* имя (Name) – произвольное имя пула сервисов
* базовый сервис (Base service) – настроенный ранее базовый сервис терминального сервера Static IP;

После сохранения настроек сервис пула открыть его двойным щелчком и добавить следующие элементы:

* группа пользователей, имеющих доступ к пулу: для добавления группы нажмите «New» и на вкладке «Groups», выберите ранее созданные аутентификатор и группу.
* протоколы подключения для пула: для добавления транспорта нажмите «New» на вкладке «Transports» созданного пула сервисов. Добавьте протокол RemoteApp.

#### **Настройка туннелированных подключений**

Для создания транспорта перейдите в соответствующий раздели панели администрирования VDI, затем New>Tunneled>выберите транспорт из списка.

![](../../../.gitbook/assets/vdi\_rds\_6.jpg)

#### Транспорт Tunneled RemoteApp

Транспорт Tunneled RemoteApp позволяет пользователям получать доступ к виртуальным приложениям Windows, **** как из внутренней, так и внешней сети. После создания транспорта задайте параметры:

Вкладка Основные:

* имя (Name) – имя транспорта;
* приоритет (Priority) – приоритет, который будет иметь транспорт. Чем ниже значение параметра, тем выше транспорт будет отображаться в списке доступных транспортов для сервиса. Транспорт с самым низким приоритетом будет использоваться по умолчанию при нажатии на иконку сервиса;

Вкладка Параметры:

* имя приложения App Name – параметр Alias (псевдоним) свойств опубликованного приложения;
* вкладка Tunnel, параметр tunnel server: укажите адрес ВМ Tunneler, в формате \<ip>:\<port> (порт по умолчанию: 443)