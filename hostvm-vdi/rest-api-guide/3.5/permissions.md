# Разрешения объектов

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение разрешений объекта системы:

**`GET /permissions/{cls}/{uuid}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{cls}` - класс объекта (сервис-провайдер, аутентификатор, транспорт, сервис-пул и т.д.)

Допустимые значения параметра `cls`:

* `providers`
* `service`
* `authenticators`
* `osmanagers`
* `transports`
* `networks`
* `servicespools`
* `calendars`
* `metapools`
* `accounts`

`{uuid}` - id объекта

### Примеры <a href="#examples" id="examples"></a>

Пример ответа со списком разрешений для сервис-провайдера:

```
[
    {
        'id': '4b409ec8-b5e5-5eae-a4f1-34b701c50bbc',
        'type': 'group',
        'auth': '750f3742-f761-5fc6-b4cc-7115bc66404b',
        'auth_name': 'int',
        'entity_id': '19b400ab-db2b-5fc7-9d7e-1e3662df594b',
        'entity_name': 'vdigroup',
        'perm': 64,
        'perm_name': 'Manage'
    },
    {
        'id': '84c4535d-4823-56c4-be5c-6c79ec779a69',
        'type': 'user',
        'auth': '750f3742-f761-5fc6-b4cc-7115bc66404b',
        'auth_name': 'int',
        'entity_id': '0f489bb0-9f70-5ee8-bb09-a30458d69fef',
        'entity_name': 'vdiuser',
        'perm': 32,
        'perm_name': 'Read'
    }
]
```

## Добавление разрешений <a href="#add-permission" id="add-permission"></a>

Добавить разрешения для объекта:

**`PUT /permissions/{cls}/{uuid}/{perm_type}/add/{entity_id}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{cls}` - класс объекта (сервис-провайдер, аутентификатор, транспорт, сервис-пул и т.д.)

Допустимые значения параметра `cls`:

* `providers`
* `service`
* `authenticators`
* `osmanagers`
* `transports`
* `networks`
* `servicespools`
* `calendars`
* `metapools`
* `accounts`

`{uuid}` - id объекта

`{perm_type}` - тип выдаваемого разрешения, на пользователя или на группу

Допустимые значения параметра `perm_type`:

* `users`
* `groups`

`{entity_id}` - id пользователя или группы (в зависимости от типа разрешения)

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`perm` - выдаваемое разрешение, тип: _string_

Допустимые значения параметра `perm`:

* `1` - чтение
* `2` - управление
