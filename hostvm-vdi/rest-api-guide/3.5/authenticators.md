# Аутентификаторы, группы и пользователи

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение списка аутентификаторов:

**`GET /authenticators`**

Получение параметров аутентификатора:

**`GET /authenticators/{auth_id}`**

Получение списка созданных в аутентификаторе групп:

**`GET /authenticators/{auth_id}/groups`**

Получение параметров группы аутентификатора:

**`GET /authenticators/{auth_id}/groups/{group_id}`**

Получение списка созданных в аутентификаторе пользователей:

**`GET /authenticators/{auth_id}/users`**

Получение параметров пользователя:

**`GET /authenticators/{auth_id}/users/{user_id}`**

Получение списка пользователей группы:

**`GET /authenticators/{auth_id}/groups/{group_id}/users`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{auth_id}` - id аутентификатора

`{group_id}` - id группы

`{user_id}` - id пользователя

### Примеры <a href="#examples" id="examples"></a>

Пример ответа с параметрами аутентификатора (Active Directory):

```
{
    'numeric_id': 14,
    'id': 'ac0702cd-cb63-5ea1-b6a6-ae3d5adff46d',
    'name': 'dc1',
    'tags': [],
    'comments': '',
    'priority': 2,
    'visible': True,
    'small_name': 'dc1',
    'users_count': 1,
    'type': 'ActiveDirectoryAuthenticator',
    'type_name': 'Active Directory Authenticator',
    'type_info': {
        'canSearchUsers': True,
        'canSearchGroups': True,
        'needsPassword': False,
        'userNameLabel': 'User name',
        'groupNameLabel': 'Group name',
        'passwordLabel': 'Password',
        'canCreateUsers': True,
        'isExternal': True
    },
    'permission': 96,
    'host': '10.1.1.1',
    'ssl': False,
    'username': 'user@domain.local',
    'password': 'password',
    'timeout': '10',
    'ldapBase': 'dc=domain,dc=local',
    'groupBase': '',
    'defaultDomain': '',
    'nestedGroups': False
}
```

Пример ответа с параметрами группы:

```
{
    'id': '71326643-3aba-5f20-9717-a09c44a5540a',
    'name': 'Domain Admins',
    'comments': '',
    'state': 'A',
    'type': 'group',
    'meta_if_any': False,
    'pools': ['d1bd5dff-5287-5afd-9444-493a3528a535']
}
```

Пример ответа с параметрами пользователя:

```
{
'name': 'vdiuser', 
'real_name': 'vdiuser', 
'comments': '', 
'state': 'A', 
'staff_member': False, 
'is_admin': False, 
'last_access': 1603712668, 
'parent': None, 
'id': 'e954f545-4870-5da2-8a95-271c0e930d3f', 
'role': 'User'
}
```

## Создание нового аутентификатора <a href="#create-auth" id="create-auth"></a>

Создание нового аутентификатора:

**`PUT /authenticators`**

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`name` - имя аутентификатора, тип: _string_

`small_name` - метка, тип: _string_

`tags` - тэги для этого элемента, тип: _list of strings_

`comments` - комментарии для этого элемента, тип: _string_

`priority` - приоритет аутентификатора, тип: _integer_

`visible` - видимость аутентификатора, тип: _boolean_

`data_type` - тип аутентификатора, тип: _string_

Допустимые значения параметра `data_type`:

* `ActiveDirectoryAuthenticator` - аутентификатор Active Directory
* `InternalDBAuth` - аутентификатор Internal Database
* `SimpleLdapAuthenticator` - аутентификатор Simple LDAP
* `RegexLdapAuthenticator` - аутентификатор Regex LDAP
* `SAML20Authenticator` - аутентификатор SAML
* `RadiusAuthenticator` - аутентификатор Radius
* `IPAuth` - аутентификатор IP

**Важно:** в зависимости от выбранного типа аутентификатора, список дополнительно требуемых параметров будет отличаться. Описание см. в [приложении](appendix.md).

## Создание группы аутентификатора <a href="#create-group" id="create-group"></a>

Создание группы аутентификатора:

**`PUT /authenticators/{auth_id}/groups`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{auth_id}` - id аутентификатора

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`type` - тип группы, тип: _string_

Допустимые значения параметра `type`:

* `group` - группа
* `meta` - метагруппа

`name` - имя группы, тип: _string_

`comments` - комментарии для этого элемента, тип: _string_

`state` - статус группы, тип: _string_. При создании используйте статус `A` (Active). Перечень остальных значений параметра `state` см. в [приложении](appendix.md).

`meta_if_any` - режим сопоставления, тип: _boolean_. Значение, отличное от `False`, учитывается только для групп типа `meta`.

`pools` - список id сервис-пулов, в которые добавлена группа, тип: _list of strings_

## Создание пользователя аутентификатора <a href="#create-user" id="create-user"></a>

Создание пользователя аутентификатора:

**`PUT /authenticators/{auth_id}/users`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{auth_id}` - id аутентификатора

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

`name` имя пользователя - , тип: _string_

`real_name` - реальное имя пользователя, тип: _string_

`comments` - комментарии для этого элемента, тип: _string_

`state` - статус пользователя, тип: _string_. При создании используйте статус `A` (Active). Перечень остальных значений параметра `state` см. в [приложении](appendix.md).

`staff_member` - признак наличия полномочий типа "Оператор", тип: _boolean_. Дополнительно см. параметр `role`.

`is_admin` - признак наличия полномочий типа "Администратор", тип: _boolean_. Дополнительно см. параметр `role`.

`groups` - список id групп, в которые добавлен пользователь, тип: _list of strings_

`role` - роль пользователя, тип: _string_

Допустимые значения параметра `role`:

* `admin` - администратор. Значения параметров `is_admin` и `staff_member` должны быть установлены в `True`.
* `staff` - оператор. Значение параметра `is_admin` должно быть установлено в `False`, параметра `staff_member` - в `True`.
* `user` - пользователь. Значения параметров `is_admin` и `staff_member` должны быть установлены в `False`.
