# Сервис-провайдеры

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение списка провайдеров:

**`GET /providers`**

Получение параметров провайдера:

**`GET /providers/{provider_id}`**

Получение списка всех созданных базовых сервисов по сервис-провайдерам:

**`GET /providers/allservices`**

Получение базовых сервисов провайдера:

**`GET /providers/{provider_id}/services`**

Получение параметров базового сервиса:

**`GET /providers/{provider_id}/services/{service_id}`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{provider_id}` - id сервис-провайдера

`{service_id}` - id базового сервиса

### Примеры <a href="#examples" id="examples"></a>

Пример ответа с параметрами провайдера:

```
{
    'id': '90e05812-dc8b-5279-a512-1aae638b3b21',
    'name': 'static',
    'tags': [],
    'services_count': 1,
    'user_services_count': 2,
    'maintenance_mode': False,
    'offers': [
        {
            'name': 'Static Multiple IP',
            'type': 'IPMachinesService',
            'description': 'This service provides access to POWERED-ON Machines by IP',
            'icon': ''
        },
        {
            'name': 'Static Single IP',
            'type': 'IPSingleMachineService',
            'description': 'This service provides access to POWERED-ON Machine by IP',
            'icon': ''
        }
    ],
    'type': 'PhysicalMachinesServiceProvider',
    'type_name': 'Static IP Machines Provider',
    'comments': '',
    'permission': 96,
    'config': ''
}
```

Пример ответа с параметрами базового сервиса:

```
{
    'id': 'afaba279-0f65-5b77-9140-12ec4cf2ff1b',
    'name': 'multiple',
    'tags': [],
    'comments': '',
    'type': 'IPMachinesService',
    'type_name': 'Static Multiple IP',
    'proxy_id': '-1',
    'proxy': '',
    'deployed_services_count': 1,
    'user_services_count': 2,
    'maintenance_mode': False,
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
    'ipList': ['10.1.141.74', '10.1.141.75'],
    'token': '',
    'port': '0',
    'skipTimeOnFailure': '0',
    'maxSessionForMachine': '0',
    'lockByExternalAccess': False
}
```

## Создание провайдера <a href="#create" id="create"></a>

Создание провайдера:

**`PUT /providers`**

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

**Обязательные:**

`name` - имя сервис-провайдера, тип: _string_

`type` - тип сервис-провайдера, тип: _string_

Допустимые значения параметра `type`:

* `oVirtPlatform` - сервис-провайдер oVirt
* `openGnsysPlatform` - сервис-провайдер OpenGnsys
* `openNebulaPlatform` - сервис-провайдер OpenNebula
* `openStackPlatform` - сервис-провайдер OpenStack(legacy)
* `openStackPlatformNew` - сервис-провайдер OpenStack
* `PhysicalMachinesServiceProvider` - сервис-провайдер Static IP
* `ProxmoxPlatform` - сервис-провайдер Proxmox
* `RDSProvider` - сервис-провайдер RDS
* `XenPlatform` - сервис-провайдер XenServer

**Важно:** в зависимости от выбранного типа провайдера, список дополнительно требуемых параметров будет отличаться. Описание см. в [приложении](appendix.md).

**Необязательные:**

`comments` - комментарии для сервис-провайдера, тип: _string_

`tags` - тэги для сервис-провайдера, тип: _list of strings_

## Создание базового сервиса провайдера <a href="#create-service" id="create-service"></a>

Создание базового сервиса провайдера:

**`PUT /providers/{provider_id}/services`**

### Параметры path <a href="#path-parameters" id="path-parameters"></a>

`{provider_id}` - id сервис-провайдера

### Параметры тела запроса <a href="#request-body-parameters" id="request-body-parameters"></a>

**Обязательные:**

`name` - имя базового сервиса, тип: _string_

`type`, `data_type` - тип базового сервиса, тип: _string_

Допустимые значения параметров `type`, `data_type`:

* `openGnsysMachine` - базовый сервис провайдера OpenGnsys
* `ProxmoxLinkedService` - базовый сервис провайдера Proxmox
* `oVirtLinkedService` - базовый сервис провайдера oVirt
* `XenLinkedService` - базовый сервис провайдера XenServer
* `openNebulaLiveService` - базовый сервис провайдера OpenNebula
* `openStackLiveService` - базовый сервис провайдера OpenStack и OpenStack(legacy)
* `IPMachinesService` - базовый сервис Static Multiple IP провайдера Static IP
* `IPSingleMachineService` - базовый сервис Static Single IP провайдера Static IP
* `RemoteSessionService` - базовый сервис Session провайдера RDS
* `RemoteAppManagedService` - базовый сервис RemoteApp провайдера RDS

**Важно:** в зависимости от выбранного типа базового сервиса, список дополнительно требуемых параметров будет отличаться. Описание см. в [приложении](appendix.md).

**Необязательные:**

`comments` - комментарии для базового сервиса, тип: _string_

`tags` - тэги для базового сервиса, тип: _list of strings_
