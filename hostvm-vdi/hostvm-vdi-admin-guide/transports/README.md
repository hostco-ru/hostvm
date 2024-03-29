# Настройка транспорта подключения

Для подключения к виртуальным рабочим столам и приложениям необходимо создать транспорт подключения.

Транспорт – это небольшое приложение, отвечающее за предоставление доступа к развернутому сервису, которое запускается на клиенте.

В зависимости от типа настроенного виртуального рабочего стола, места расположения и способа подключения, могут использоваться различные типы транспортов.

На клиенте и ВМ должны быть установлены необходимые компоненты ПО для протокола соединения, используемого в качестве транспорта.

Прямое подключение (транспорт) используется для доступа пользователей к виртуальным рабочим столам и приложениям из внутренней локальной сети, VPN, LAN Extension и т. д.

![](../../../.gitbook/assets/vdi\_ag26.png)

Туннелированное подключение (транспорт) используется для доступа пользователей к виртуальным рабочим столам и приложениям из глобальной сети. Транспорты этого типа используют HOSTVM VDI туннелер для установления соединения.

![](../../../.gitbook/assets/vdi\_ag25.png)

Транспорт HTML5 может использоваться для доступа пользователей к виртуальным рабочим столам с помощью всех типов доступа (LAN, WAN и т.д.). Этот транспорт использует HOSTVM VDI туннелер для соединения с виртуальными рабочими столами.

Для создания нового транспорта нажмите «New» в разделе «Connectivity» -> «Transports».

Примечания:

* если вам нужен доступ из сети, которая не является вашей локальной сетью, используйте туннелированные транспорты. Для их настройки потребуется общедоступный IP-адрес сервера HOSTVM VDI Tunneler;
* HOSTVM VDI Tunneler использует один из двух портов в зависимости от типа транспорта. По умолчанию, при соединении через HTML5 используется порт 10443, для остальных туннелированных соединений (RDP, RDS, X2Go и т.д.) - порт 443.

