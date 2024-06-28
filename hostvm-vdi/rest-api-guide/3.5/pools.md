# Сервис-пулы

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение списка сервис-пулов и их параметров:

**`GET /servicespools/overview`**

Получение параметров пула:

**`GET /servicespools/{pool_id}`**

Получение списка групп, добавленных в пул:

**`GET /servicespools/{pool_id}/groups`**

Получение списка транспортов, добавленных в пул:

**`GET /servicespools/{pool_id}/transports`**

Получение списка сервисов пула (тонких клонов, виртуальных машин, и т.д.), назначенных пользователям (закрепленных за ними):

**`GET /servicespools/{pool_id}/services`**

Получение списка сервисов пула, находящихся в кэше (развернутые, но не назначенные пользователям экземпляры):

**`GET /servicespools/{pool_id}/cache`**

Получение списка назначаемых сервисов:

**`GET /servicespools/{pool_id}/listAssignables`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{pool_id}` - id пула

### Примеры <a href="#examples" id="examples"></a>

Пример ответа с параметрами пула:

```
{
    'id': 'd1bd5dff-5287-5afd-9444-493a3528a535',
    'name': 'static-mult',
    'short_name': '',
    'tags': [],
    'parent': 'multiple',
    'parent_type': 'IPMachinesService',
    'comments': '',
    'state': 'A',
    'thumb': '',
    'account': '',
    'account_id': None,
    'service_id': 'afaba279-0f65-5b77-9140-12ec4cf2ff1b',
    'provider_id': '90e05812-dc8b-5279-a512-1aae638b3b21',
    'image_id': None,
    'initial_srvs': 0,
    'cache_l1_srvs': 0,
    'cache_l2_srvs': 0,
    'max_srvs': 0,
    'show_transports': True,
    'visible': True,
    'allow_users_remove': False,
    'allow_users_reset': False,
    'ignores_unused': False,
    'fallbackAccess': 'ALLOW',
    'meta_member': [],
    'calendar_message': '',
    'user_services_count': 1,
    'user_services_in_preparation': 0,
    'restrained': False,
    'permission': 96,
    'info': {
        'icon': '',
        'needs_publication': False,
        'max_deployed': -1,
        'uses_cache': False,
        'uses_cache_l2': False,
        'cache_tooltip': 'None',
        'cache_tooltip_l2': 'None',
        'needs_manager': False,
        'allowedProtocols': ['rdp', 'rgs', 'vnc', 'nx', 'x11', 'x2go', 'pcoip', 'nicedcv', 'other'],
        'servicesTypeProvided': ['vdi'],
        'must_assign_manually': False,
        'can_reset': False,
        'can_list_assignables': True
    },
    'pool_group_id': None,
    'pool_group_name': 'Default',
    'pool_group_thumb': '',
    'usage': '100%'
}
```

Пример ответа со списком добавленных в пул групп:

```
[
    {
        'id': '71326643-3aba-5f20-9717-a09c44a5540a',
        'auth_id': 'ac0702cd-cb63-5ea1-b6a6-ae3d5adff46d',
        'name': 'Domain Admins',
        'group_name': 'Domain Admins@dc1',
        'comments': '',
        'state': 'A',
        'type': 'group',
        'auth_name': 'dc1'
    },
    {
        'id': '19b400ab-db2b-5fc7-9d7e-1e3662df594b',
        'auth_id': '750f3742-f761-5fc6-b4cc-7115bc66404b',
        'name': 'vdigroup',
        'group_name': 'vdigroup@int',
        'comments': '',
        'state': 'A',
        'type': 'group',
        'auth_name': 'int'
    },
    {
        'id': 'de14df32-69d8-5add-96c5-be758f2ca49c',
        'auth_id': '750f3742-f761-5fc6-b4cc-7115bc66404b',
        'name': 'vdigroup1',
        'group_name': 'vdigroup1@int',
        'comments': '',
        'state': 'A',
        'type': 'group',
        'auth_name': 'int'
    }
]
```

Пример ответа со списком добавленных в пул транспортов:

```
[
    {
        'id': 'b925c748-c3cc-585e-a8ec-9d7dd3a2c649',
        'name': 'rdp-direct',
        'type': {
            'name': 'RDP',
            'type': 'RDPTransport',
            'description': 'RDP Protocol. Direct connection.',
            'icon': '',
            'group': 'Direct'
        },
        'comments': '',
        'priority': 1,
        'trans_type': 'RDP'
    },
    {
        'id': 'c532143e-86be-500b-9236-a449825403b2',
        'name': 'x2go-direct',
        'type': {
            'name': 'X2Go',
            'type': 'X2GOTransport',
            'description': 'X2Go access (Experimental). Direct connection.',
            'icon': '',
            'group': 'Direct'
        },
        'comments': '',
        'priority': 2,
        'trans_type': 'X2Go'
    }
]
```

Пример ответа со списком назначенных пользователям сервисов пула:

```
[
    {
        'id': 'b1931a7d-3a2c-5d8a-9d8f-d517188ac5eb',
        'id_deployed_service': 'd1bd5dff-5287-5afd-9444-493a3528a535',
        'unique_id': '10.1.141.74',
        'friendly_name': '10.1.141.74',
        'state': 'U',
        'os_state': 'U',
        'state_date': 1707128501,
        'creation_date': 1707128500,
        'revision': '',
        'ip': '10.1.141.74',
        'actor_version': 'unknown',
        'owner': 'vdiuser@int',
        'owner_info': {'auth_id': '750f3742-f761-5fc6-b4cc-7115bc66404b', 'user_id': '0f489bb0-9f70-5ee8-bb09-a30458d69fef'},
        'in_use': True,
        'in_use_date': 1707128500,
        'source_host': 'hostname',
        'source_ip': '10.1.44.13'
    },
    {
        'id': '9599b697-8b4e-5ca1-98f8-88aff03a227d',
        'id_deployed_service':
        'd1bd5dff-5287-5afd-9444-493a3528a535',
        'unique_id': '10.1.141.76',
        'friendly_name': '10.1.141.76',
        'state': 'U',
        'os_state': 'U',
        'state_date': 1707985180,
        'creation_date': 1707985180,
        'revision': '',
        'ip': '10.1.141.76',
        'actor_version': 'unknown',
        'owner': 'adm@hostvm.local@dc1',
        'owner_info': {'auth_id': 'ac0702cd-cb63-5ea1-b6a6-ae3d5adff46d', 'user_id': '7c2aec6f-3824-5a8b-8a80-0bb5e6ddbea9'},
        'in_use': True,
        'in_use_date': 1707985180,
        'source_host': '',
        'source_ip': ''
    }
]
```

Пример ответа со списком сервисов в кэше:

```
[
    {
        'id': 'b2b9c5ba-0943-58f3-ba06-05b0c2738028', 
        'id_deployed_service': '9a801202-eb99-5f75-9137-c64aad365e1f', 
        'unique_id': '52:54:00:00:00:01', 
        'friendly_name': 'vdi3-alt-00', 
        'state': 'U', 
        'os_state': 'U', 
        'state_date': 1631271805, 
        'creation_date': 1631271750, 
        'revision': 3, 
        'ip': '10.1.2.13', 
        'actor_version': '2.2.0', 
        'cache_level': 1
    }
]
```

## Создание сервис-пула <a href="#create-pool" id="create-pool"></a>

Создание нового пула:

**`PUT /servicespools`**

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`name` - имя пула, тип: _string_

`service_id` - id базового сервиса, тип: _string_

`osmanager_id` - id менеджера ОС, тип: _string_. При создании пула на основе сервиса, не использующего менеджер ОС (например Static IP), используйте значение `None`.

`short_name` - короткое имя, тип: _string_

`comments` - комментарии для этого элемента, тип: _string_

`tags` - тэги для этого элемента, тип: _list of strings_

`image_id` - id изображения для пула, тип: _string_. Для изображения по умолчанию используйте значение `-1`.

`pool_group_id` - id группы пулов, тип: _string_. Для группы по умолчанию используйте значение `-1`.

`visible` - видимость пула, тип: _boolean_

`calendar_message` - сообщение при ограничении доступа по календарю, тип: _string_

`allow_users_remove` - разрешить удаление пользователями, тип: _boolean_

`allow_users_reset` - разрешить сброс пользователями, тип: _boolean_

`ignores_unused` - игнорировать неиспользуемые, тип: _boolean_

`show_transports` - показать транспорты, тип: _boolean_

`account_id` - учет использования, тип: _string_. Для настройки по умолчанию используйте значение `-1`.

`initial_srvs` - первоначально доступные сервисы, тип: _integer_. При создании пула на основе сервиса, не использующего менеджер ОС (например Static IP), используйте значение `0`.

`cache_l1_srvs` - сервисы, хранящиеся в кэше, тип: _integer_. При создании пула на основе сервиса, не использующего менеджер ОС (например Static IP), используйте значение `0`.

`cache_l2_srvs` - сервисы, хранящиеся в L2 кэше, тип: _integer_. При создании пула на основе сервиса, не использующего менеджер ОС (например Static IP), используйте значение `0`.

`max_srvs` - максимальное количество предоставляемых сервисов, тип: _integer_. При создании пула на основе сервиса, не использующего менеджер ОС (например Static IP), используйте значение `0`.

### Примеры <a href="#examples" id="examples"></a>

Пример выдачи:

```
[
{
'id': 'ee27235f-59c3-5407-b897-d1c9a7ed84e0', 
'name': 'AltLinux', 
'tags': [], 
'comments': '', 
'type': 'oVirtLinkedService', 
'type_name': 'oVirt/RHEV Linked Clone', 
'proxy_id': '-1', 
'proxy': '', 
'deployed_services_count': 2, 
'user_services_count': 2, 
'maintenance_mode': False, 
'permission': 96, 
'info': {'icon': '...', 'needs_publication': True, 'max_deployed': -1, 'uses_cache': True, 'uses_cache_l2': True, 'cache_tooltip': 'Number of desired machines to keep running waiting for a user', 'cache_tooltip_l2': 'Number of desired machines to keep suspended waiting for use', 'needs_manager': True, 'allowedProtocols': ['rdp', 'rgs', 'vnc', 'nx', 'x11', 'x2go', 'pcoip', 'other', 'spice'], 'servicesTypeProvided': ['vdi'], 'must_assign_manually': False, 'can_reset': True, 'can_list_assignables': False}
}, 
... 
]
```

## &#x20;<a href="#edit-pool" id="edit-pool"></a>

## Изменение конфигурации сервис-пула <a href="#edit-pool" id="edit-pool"></a>

Изменение параметров пула:

**`PUT /servicespools/{pool_id}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`pool_id` - id сервис-пула

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`name` - имя пула, тип: _string_

`short_name` - короткое имя, тип: _string_

`comments` - комментарии для этого элемента, тип: _string_

`tags` - тэги для этого элемента, тип: _list of strings_

`image_id` - id изображения для пула, тип: _string_. Для изображения по умолчанию используйте значение `-1`.

`pool_group_id` - id группы пулов, тип: _string_. Для группы по умолчанию используйте значение `-1`.

`visible` - видимость пула, тип: _boolean_

`calendar_message` - сообщение при ограничении доступа по календарю, тип: _string_

`allow_users_remove` - разрешить удаление пользователями, тип: _boolean_

`allow_users_reset` - разрешить сброс пользователями, тип: _boolean_

`ignores_unused` - игнорировать неиспользуемые, тип: _boolean_

`show_transports` - показать транспорты, тип: _boolean_

`account_id` - учет использования, тип: _string_. Для настройки по умолчанию используйте значение `-1`.

`initial_srvs` - первоначально доступные сервисы, тип: _integer_. Для пула на основе сервиса, не использующего менеджер ОС (например Static IP), значение параметра равно `0` и не модифицируется.

`cache_l1_srvs` - сервисы, хранящиеся в кэше, тип: _integer_. Для пула на основе сервиса, не использующего менеджер ОС (например Static IP), значение параметра равно `0` и не модифицируется.

`cache_l2_srvs` - сервисы, хранящиеся в L2 кэше, тип: _integer_. Для пула на основе сервиса, не использующего менеджер ОС (например Static IP), значение параметра равно `0` и не модифицируется.

`max_srvs` - максимальное количество предоставляемых сервисов, тип: _integer_. Для пула на основе сервиса, не использующего менеджер ОС (например Static IP), значение параметра равно `0` и не модифицируется.

## Удаление сервис-пула <a href="#delete-pool" id="delete-pool"></a>

Удаление сервис-пула:

**`DELETE /servicespools/{pool_id}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`pool_id` - id сервис-пула

## Добавление группы в сервис-пул <a href="#add-group" id="add-group"></a>

Добавление в пул группы доступа:

**`PUT /servicespools/{pool_id}/groups`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`pool_id` - id сервис-пула

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`id` - id группы, тип: _string_

## Добавление транспорта в сервис-пул <a href="#add-transport" id="add-transport"></a>

Добавление транспорта в сервис-пул:

**`PUT /servicespools/{pool_id}/transports`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`pool_id` - id сервис-пула

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`id` - id транспорта, тип: _string_

## Публикация сервисов пула <a href="#publish" id="publish"></a>

Публикация сервисов пула:

**`GET /servicespools/{pool_id}/publications/publish`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`pool_id` - id сервис-пула

В случае успешного запуска процесса публикации возвращается ответ `ok`.

## Назначение сервисов <a href="#assign" id="assign"></a>

Назначить пользователю экземпляр сервиса пула:

**`GET /servicespools/{pool_id}/createFromAssignable`**

> доступно только для сервисов, поддерживающих ручное назначение пользователям (Static IP провайдер - Multiple IP сервис)

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{pool_id}` - id сервис-пула

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`user_id` - id пользователя, тип: _string_

`assignable_id` - id назначаемого сервиса пула, тип: _string_

Для получения `assignable_id` необходимо запросить список назначаемых сервисов пула, см. соответствующий метод в разделе `Сервис-пулы / Запрос текущей конфигурации`.
