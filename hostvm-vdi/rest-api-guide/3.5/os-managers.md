# Менеджеры ОС

## Запрос текущей конфигурации <a href="#get-config" id="get-config"></a>

Получение списка менеджеров ОС:

**`GET /osmanagers`**

### Примеры <a href="#examples" id="examples"></a>

Пример ответа со списком менеджеров ОС:

```
[
{'id': '6a2c5205-b888-5e7b-83eb-8fc32787e718', 'name': 'Linux', 'tags': [], 'deployed_count': 6, 'type': 'LinuxManager', 'type_name': 'Linux OS Manager', 'servicesTypes': ['vdi'], 'comments': '', 'permission': 96}, 
{'id': '05dc012f-1ab8-51f4-9fce-b321f5945b64', 'name': 'WinBasic', 'tags': [], 'deployed_count': 1, 'type': 'WindowsManager', 'type_name': 'Windows Basic OS Manager', 'servicesTypes': ['vdi'], 'comments': '', 'permission': 96}
]
```
