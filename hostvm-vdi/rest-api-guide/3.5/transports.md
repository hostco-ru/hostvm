# Транспорты

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение списка созданных транспортов:

**`GET /transports`**

Получение параметров транспорта:

**`GET /transports/{transport_id}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{transport_id}` - id транспорта

### Примеры <a href="#examples" id="examples"></a>

Пример ответа с параметрами транспорта (RDP direct):

```
{
    'id': 'b925c748-c3cc-585e-a8ec-9d7dd3a2c649',
    'name': 'rdp-direct',
    'tags': [],
    'comments': '',
    'priority': 1,
    'nets_positive': True,
    'label': '',
    'networks': [],
    'allowed_oss': [],
    'pools': [{'id': '4302fdcb-a692-5262-93d0-ab460bbd1f84'}, {'id': 'd1bd5dff-5287-5afd-9444-493a3528a535'}],
    'pools_count': 2,
    'deployed_count': 2,
    'type': 'RDPTransport',
    'type_name': 'RDP',
    'protocol': 'rdp',
    'permission': 96,
    'useEmptyCreds': False,
    'fixedName': '',
    'fixedPassword': '',
    'withoutDomain': False,
    'fixedDomain': '',
    'allowSmartcards': False,
    'allowPrinters': False,
    'allowDrives': False,
    'enforceDrives': '',
    'allowSerials': False,
    'allowClipboard': True,
    'allowAudio': True,
    'allowWebcam': False,
    'usbRedirection': False,
    'wallpaper': False,
    'multimon': False,
    'aero': False,
    'smooth': True,
    'showConnectionBar': True,
    'credssp': True,
    'rdpPort': '3389',
    'screenSize': '-1x-1',
    'colorDepth': '24',
    'alsa': False,
    'multimedia': False,
    'printerString': '',
    'smartcardString': '',
    'customParameters': '/sec:tls',
    'allowMacMSRDC': False,
    'customParametersMAC': ''
}
```

## Создание нового транспорта <a href="#create-transport" id="create-transport"></a>

Создание нового транспорта:

**`PUT /transports`**

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`name` - имя транспорта, тип: _string_

`tags` - тэги для этого элемента, тип: _list of strings_

`comments` - комментарии для этого элемента, тип: _string_

`priority` - приоритет транспорта, тип: _integer_

`nets_positive` - сетевой доступ, тип: _boolean_

`label` - метка, тип: _string_

`networks` - список id сетей, ассоциированных с транспортом, тип: _list of strings_

`allowed_oss` - список разрешенных устройств, тип: _list of strings_

Перечень значений параметра `allowed_oss` (допустимо указывать несколько значений, либо передать пустую строку(без ограничений по устройствам)):

* `WindowsPhone`
* `Android`
* `Linux`
* `Windows`
* `iPad`
* `iPhone`
* `Macintosh`
* `ChromeOS`
* `WYSE`

`pools` - список id сервис-пулов, в которые добавлен транспорт, тип: _list of strings_

`type` - тип транспорта, тип: _string_

Допустимые значения параметра `type`:

* `RDPTransport` - транспорт RDP direct
* `TSRDPTransport` - транспорт RDP tunnel
* `RATransport` - транспорт RemoteApp direct
* `TRATransport` - транспорт RemoteApp tunnel
* `HTML5RDPTransport` - транспорт HTML5 RDP
* `HTML5RATransport` - транспорт HTML5 RemoteApp
* `PCoIPTransport` - транспорт PCoIP Cloud Access
* `SPICETransport` - транспорт SPICE direct
* `TSSPICETransport` - транспорт SPICE tunnel
* `URLTransport` - транспорт URL Launcher
* `X2GOTransport` - транспорт X2Go direct
* `TX2GOTransport` - транспорт X2Go tunnel

**Важно:** в зависимости от выбранного типа транспорта, список дополнительно требуемых параметров будет отличаться. Описание см. в [приложении](appendix.md).

## Добавление сетей <a href="#add-network" id="add-network"></a>

Добавление сетей в транспорт:

**`PUT /transports/{transport_id}/networks`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{transport_id}` - id транспорта

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

**Обязательные:**

`id` - id добавляемой сети, тип: _string_
