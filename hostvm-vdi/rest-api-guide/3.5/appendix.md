# Приложение

## Статус объектов <a href="#object-states" id="object-states"></a>

Перечень всех возможных статусов объектов VDI (параметр `state`). Не все объекты поддерживают все статусы.

```
    'A' - Active
    'B' - Blocked
    'C' - Canceled
    'E' - Error
    'F' - Finished
    'H' - Balancing
    'I' - Inactive
    'J' - Too many preparing services
    'K' - Canceling
    'L' - Waiting publication
    'M' - Removing
    'P' - In preparation
    'R' - Removable
    'S' - Removed
    'T' - Restrained
    'U' - Valid
    'W' - Running
    'X' - Waiting execution
    'Y' - In maintenance
    'Z' - Waiting OS
    'V' - Meta member
```

## Параметры провайдеров и базовых сервисов <a href="#provider-parameters" id="provider-parameters"></a>

### PhysicalMachinesServiceProvider

**Необязательные:**

`config` - расширенная конфигурация, тип: _string_

#### **IPMachinesService**

`ipList` - список серверов, тип: _list of strings_

`token` - токен сервиса, тип: _string_

`port` - порт для проверки соединения, тип: _integer_

`skipTimeOnFailure` - интервал между проверками соединения при неудаче, тип: _integer_

`maxSessionForMachine` - максимальная продолжительность сеанса, в часах, тип: _integer_

`lockByExternalAccess` - блокировать машину при внешнем доступе, тип: _boolean_

`proxy_id` - id прокси, тип: _string_. Если отсутствует, используйте значение `-1`.

## Параметры аутентификаторов <a href="#authenticator-parameters" id="authenticator-parameters"></a>

### ActiveDirectoryAuthenticator

`host` - IP или FQDN сервера AD, тип: _string_

`username` - имя пользователя, тип: _string_

`password` - пароль, тип: _string_

`ldapBase` - база поиска, тип: _string_

`ssl` - использовать SSL, тип: _string_. Допустимые значения: `true`, `false`.

`timeout` - таймаут подключения, тип: _string_

`groupBase` - база поиска для групп, тип: _string_

`defaultDomain` - домен по умолчанию, тип: _string_

`nestedGroups` - получить вложенные группы, тип: _string_. Допустимые значения: `true`, `false`.

### InternalDBAuth

`differentForEachHost` - разные пользователи для каждого хоста, тип: _string_. Допустимые значения: `true`, `false`.

`reverseDns` - обратный DNS, тип: _string_. Допустимые значения: `true`, `false`.

`acceptProxy` - разрешить прокси, тип: _string_. Допустимые значения: `true`, `false`.

### RegexLdapAuthenticator

`host` - адрес или имя LDAP сервера, тип: _string_

`username` - имя пользователя, тип: _string_

`password` - пароль, тип: _string_

`ldapBase` - база поиска, тип: _string_

`port` - порт LDAP сервера, тип: _string_

`ssl` - использовать SSL, тип: _string_. Допустимые значения: `true`, `false`.

`timeout` - таймаут подключения, тип: _string_

`userClass` - класс для пользователей LDAP, тип: _string_

`userIdAttr` - атрибут идентификатора пользователя, тип: _string_

`groupNameAttr` - атрибуты имени группы, тип: _string_

`userNameAttr` - атрибут имени пользователя, тип: _string_

`altClass` - альтернативный класс для поиска групп, тип: _string_

### SimpleLdapAuthenticator

`host` - адрес или имя LDAP сервера, тип: _string_

`username` - имя пользователя, тип: _string_

`password` - пароль, тип: _string_

`ldapBase` - база поиска, тип: _string_

`port` - порт LDAP сервера, тип: _string_

`ssl` - использовать SSL, тип: _string_. Допустимые значения: `true`, `false`.

`timeout` - таймаут подключения, тип: _string_

`userClass` - класс для пользователей LDAP, тип: _string_

`groupClass` - класс для групп LDAP, тип: _string_

`userIdAttr` - атрибут идентификатора пользователя, тип: _string_

`groupIdAttr` - атрибут идентификатора группы, тип: _string_

`memberAttr` - атрибут членства в группе, тип: _string_

`userNameAttr` - атрибут имени пользователя, тип: _string_

### SAML20Authenticator

`privateKey` - приватный ключ, тип: _string_

`serverCertificate` - сертификат, тип: _string_

`idpMetadata` - метаданные IDP, тип: _string_

`userNameAttr` - атрибут имени пользователя, тип: _string_

`groupNameAttr` - атрибуты имени группы, тип: _string_

`realNameAttr` - атрибуты реального имени пользователя, тип: _string_

`entityID` - идентификатор SP, тип: _string_

`usePassword` - использовать пароль, тип: _string_. Допустимые значения: `true`, `false`.

`pwdAttr` - атрибут пароля, тип: _string_

## Параметры транспортов <a href="#transport-parameters" id="transport-parameters"></a>

### RDPTransport

`useEmptyCreds` - пропустить данные аккаунта, тип: _string_. Допустимые значения: `true`, `false`.

`fixedName` - имя пользователя, тип: _string_

`fixedPassword` - пароль, тип: _string_

`withoutDomain` - без домена, тип: _string_. Допустимые значения: `true`, `false`.

`fixedDomain` - домен, тип: _string_

`allowSmartcards` - разрешить смарткарты, тип: _string_. Допустимые значения: `true`, `false`.

`allowPrinters` - разрешить принтеры, тип: _string_. Допустимые значения: `true`, `false`.

`allowDrives` - политика локальных дисков, тип: _string_. Допустимые значения: `true` - allow any, `dynamic` - allow PnP, `false` - allow none.

`enforceDrives` - принудительное подключение дисков, тип: _string_

`allowSerials` - разрешить серийные порты, тип: _string_. Допустимые значения: `true`, `false`.

`allowClipboard` - включить буфер обмена, тип: _string_. Допустимые значения: `true`, `false`.

`allowAudio` - включить звук, тип: _string_. Допустимые значения: `true`, `false`.

`allowWebcam` - включить веб-камеру, тип: _string_. Допустимые значения: `true`, `false`.

`usbRedirection` - проброс USB, тип: _string_.

Допустимые значения параметра `usbRedirection`:

* `true` - Allow all
* `false` - Allow none
* `{ca3e7ab9-b4c3-4ae6-8251-579ef933890f}` - Cameras
* `{4d36e967-e325-11ce-bfc1-08002be10318}` - Disk Drives
* `{4d36e979-e325-11ce-bfc1-08002be10318}` - Printers
* `{50dd5230-ba8a-11d1-bf5d-0000f805f530}` - Smartcards
* `{745a17a0-74d3-11d0-b6fe-00a0c90f57da}` - HIDs

`wallpaper` - обои/темы, тип: _string_. Допустимые значения: `true`, `false`.

`multimon` - несколько мониторов, тип: _string_. Допустимые значения: `true`, `false`.

`aero` - разрешить композицию рабочего стола, тип: _string_. Допустимые значения: `true`, `false`.

`smooth` - сглаживание шрифтов, тип: _string_. Допустимые значения: `true`, `false`.

`showConnectionBar` - панель подключения, тип: _string_. Допустимые значения: `true`, `false`.

`credssp` - поддержка Credssp, тип: _string_. Допустимые значения: `true`, `false`.

`rdpPort` - порт RDP, тип: _string_

`screenSize` - размер экрана, тип: _string_. Для полноэкранного режима используйте значение `-1x-1`

`colorDepth` - глубина цвета, тип: _string_

`alsa` - использовать Alsa, тип: _string_. Допустимые значения: `true`, `false`.

`multimedia` - оптимизация видеопотока, тип: _string_. Допустимые значения: `true`, `false`.

`printerString` - строка принтера, тип: _string_

`smartcardString` - строка Smartcard, тип: _string_

`customParameters` - дополнительные параметры Linux, тип: _string_

`allowMacMSRDC` - разрешить клиент Microsoft RDP, тип: _string_. Допустимые значения: `true`, `false`.

`customParametersMAC` - дополнительные параметры MacOS, тип: _string_

### TSRDPTransport

`tunnelServer` - туннельный сервер, тип: _string_

`tunnelWait` - время ожидания туннеля, тип: _string_

`verifyCertificate` - проверка SSL-сертификата, тип: _string_. Допустимые значения: `true`, `false`.

> Остальные параметры аналогичны типу транспорта `RDPTransport`.

### RATransport

`fixedApp` - алиас приложения, тип: _string_

> Остальные параметры аналогичны типу транспорта `RDPTransport`.

### TRATransport

`fixedApp` - алиас приложения, тип: _string_

> Остальные параметры аналогичны типу транспорта `TSRDPTransport`.

### HTML5RDPTransport

`guacamoleServer` - туннельный сервер, тип: _string_

`useGlyptodonTunnel` - использовать туннель Glyptodon Enterprise, тип: _string_. Допустимые значения: `true`, `false`.

`useEmptyCreds` - пропустить данные аккаунта, тип: _string_. Допустимые значения: `true`, `false`.

`fixedName` - имя пользователя, тип: _string_

`fixedPassword` - пароль, тип: _string_

`withoutDomain` - без домена, тип: _string_. Допустимые значения: `true`, `false`.

`fixedDomain` - домен, тип: _string_

`wallpaper` - показать обои, тип: _string_. Допустимые значения: `true`, `false`.

`desktopComp` - разрешить композицию рабочего стола, тип: _string_. Допустимые значения: `true`, `false`.

`smooth` - сглаживание шрифтов, тип: _string_. Допустимые значения: `true`, `false`.

`enableAudio` - включить аудио, тип: _string_. Допустимые значения: `true`, `false`.

`enableAudioInput` - включить микрофон, тип: _string_. Допустимые значения: `true`, `false`.

`enablePrinting` - включить печать, тип: _string_. Допустимые значения: `true`, `false`.

`enableFileSharing` - обмен файлами, тип: _string_. Допустимые значения: `true`, `false`.

Допустимые значения параметра `enableFileSharing`:

* `false` - Disable file sharing
* `down` - Allow download only
* `up` - Allow upload only
* `true` - Enable file sharing

`enableClipboard` - буфер обмена, тип: _string_

Допустимые значения параметра `enableClipboard`:

* `disabled` - Disable clipboard
* `dis-copy` - Disable copy from remote
* `dis-paste` - Disable paste to remote
* `enabled` - Enable clipboard

`serverLayout` - раскладка, тип: _string_. Значение по умолчанию (default): `-`

`ticketValidity` - срок действия тикета, тип: _string_

`forceNewWindow` - открывать в новом окне, тип: _string_. Допустимые значения: `true`, `false`.

Допустимые значения параметра `forceNewWindow`:

* `false` - Open every connection on the same window, but keeps UDS window
* `true` - Force every connection to be opened on a new window
* `overwrite` - Override UDS window and replace it with the connection

`security` - безопасность, тип: _string_

Допустимые значения параметра `security`:

* `any` - Any (Allow the server to choose the type of auth)
* `rdp` - RDP (Standard RDP encryption. Should be supported by all servers)
* `nla` - NLA (Network Layer authentication. Requires VALID username\&password, or connection will fail)
* `nla-ext` - NLA extended (Network Layer authentication. Requires VALID username\&password, or connection will fail)
* `tls` - TLS (Transport Security Layer encryption)

`rdpPort` - порт RDP, тип: _string_

`customGEPath` - контекстный путь Glyptodon Enterprise, тип: _string_

### HTML5RATransport

`guacamoleServer` - туннельный сервер, тип: _string_

`useEmptyCreds` - пропустить данные аккаунта, тип: _string_. Допустимые значения: `true`, `false`.

`fixedName` - имя пользователя, тип: _string_

`fixedPassword` - пароль, тип: _string_

`withoutDomain` - без домена, тип: _string_. Допустимые значения: `true`, `false`.

`fixedDomain` - домен, тип: _string_

`fixedApp` - алиас приложения, тип: _string_

`wallpaper` - показать обои, тип: _string_. Допустимые значения: `true`, `false`.

`desktopComp` - разрешить композицию рабочего стола, тип: _string_. Допустимые значения: `true`, `false`.

`smooth` - сглаживание шрифтов, тип: _string_. Допустимые значения: `true`, `false`.

`enableAudio` - включить аудио, тип: _string_. Допустимые значения: `true`, `false`.

`enableAudioInput` - включить микрофон, тип: _string_. Допустимые значения: `true`, `false`.

`enablePrinting` - включить печать, тип: _string_. Допустимые значения: `true`, `false`.

`enableFileSharing` - обмен файлами, тип: _string_. Допустимые значения: `true`, `false`.

Допустимые значения параметра `enableFileSharing`:

* `false` - Disable file sharing
* `down` - Allow download only
* `up` - Allow upload only
* `true` - Enable file sharing

`enableClipboard` - буфер обмена, тип: _string_

Допустимые значения параметра `enableClipboard`:

* `disabled` - Disable clipboard
* `dis-copy` - Disable copy from remote
* `dis-paste` - Disable paste to remote
* `enabled` - Enable clipboard

`serverLayout` - раскладка, тип: _string_. Значение по умолчанию (default): `-`

`ticketValidity` - срок действия тикета, тип: _string_

`forceNewWindow` - открывать в новом окне, тип: _string_. Допустимые значения: `true`, `false`.

Допустимые значения параметра `forceNewWindow`:

* `false` - Open every connection on the same window, but keeps UDS window
* `true` - Force every connection to be opened on a new window
* `overwrite` - Override UDS window and replace it with the connection

`security` - безопасность, тип: _string_

Допустимые значения параметра `security`:

* `any` - Any (Allow the server to choose the type of auth)
* `rdp` - RDP (Standard RDP encryption. Should be supported by all servers)
* `nla` - NLA (Network Layer authentication. Requires VALID username\&password, or connection will fail)
* `nla-ext` - NLA extended (Network Layer authentication. Requires VALID username\&password, or connection will fail)
* `tls` - TLS (Transport Security Layer encryption)

`rdpPort` - порт RDP, тип: _string_

### PCoIPTransport

`fixedName` - имя пользователя, тип: _string_

`fixedPassword` - пароль, тип: _string_

`fixedDomain` - домен, тип: _string_

### SPICETransport

`serverCertificate` - сертификат, тип: _string_

`fullScreen` - полноэкранный режим, тип: _string_. Допустимые значения: `true`, `false`.

`usbShare` - включить USB, тип: _string_. Допустимые значения: `true`, `false`.

`autoNewUsbShare` - автоподключение новых USB устройств, тип: _string_. Допустимые значения: `true`, `false`.

`smartCardRedirect` - перенаправление смарт-карты, тип: _string_. Допустимые значения: `true`, `false`.

### TSSPICETransport

`tunnelServer` - туннельный сервер, тип: _string_

`tunnelWait` - время ожидания туннеля, тип: _string_

`verifyCertificate` - проверка SSL-сертификата, тип: _string_. Допустимые значения: `true`, `false`.

> Остальные параметры аналогичны типу транспорта `SPICETransport`.

### URLTransport

`urlPattern` - шаблон URL, тип: _string_

`forceNewWindow` - открывать в новом окне, тип: _string_. Допустимые значения: `true`, `false`.

### X2GOTransport

`fixedName` - имя пользователя, тип: _string_

`screenSize` - размер экрана, тип: _string_. Для полноэкранного режима используйте значение `F`

`desktopType` - рабочий стол, тип: _string_

Допустимые значения параметра `desktopType`:

* `XFCE` - Xfce
* `MATE` - Mate
* `LXDE` - Lxde
* `GNOME` - Gnome (see docs)
* `KDE` - Kde (see docs)
* `gnome-session-cinnamon` - Cinnamon 1.4 (see docs)
* `gnome-session-cinnamon2d` - Cinnamon 2.2 (see docs)
* `UDSVAPP` - UDS vAPP

`customCmd` - строка vAPP, тип: _string_

`sound` - включить звук, тип: _string_. Допустимые значения: `true`, `false`.

`exports` - перенаправить домашнюю папку, тип: _string_. Допустимые значения: `true`, `false`.

`speed` - скорость, тип: _string_

Допустимые значения параметра `speed`:

* `0` - MODEM
* `1` - ISDN
* `2` - ADSL
* `3` - WAN
* `4` - LAN

`soundType` - звук, тип: _string_

Допустимые значения параметра `soundType`:

* `pulse` - Pulse
* `esd` - ESD

`keyboardLayout` - клавиатура, тип: _string_

`pack` - метод сжатия, тип: _string_. Значение по умолчанию: `16m-jpeg`

`quality` - качество, тип: _string_

### TX2GOTransport

`tunnelServer` - туннельный сервер, тип: _string_

`tunnelWait` - время ожидания туннеля, тип: _string_

`verifyCertificate` - проверка SSL-сертификата, тип: _string_. Допустимые значения: `true`, `false`.

> Остальные параметры аналогичны типу транспорта `X2GOTransport`.
