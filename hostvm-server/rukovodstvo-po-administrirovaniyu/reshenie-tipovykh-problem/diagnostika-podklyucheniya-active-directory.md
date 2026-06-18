---
description: Данная статья описывает способы диагностики интеграции с Active Directory
---

# Диагностика подключения Active Directory

### Проверка подключения к LDAP-серверу

Предварительно установите пакет openldap-clients:

```
yum install openldap-clients
```

Выполните запрос по поиску пользователя в каталоге:

```
ldapsearch -H ldap://ldap_server.domain.ru:389 -x -D "CN=Администратор,CN=Users,DC=domain,DC=ru" -W -b "DC=domain,DC=ru" "(sAMAccountName=username)"
```

-D "CN=Администратор,CN=Users,DC=domain,DC=ru" – учетная запись для аутентификации, имя, контейнер, домен\
-b "DC=domain,DC=ru" – начальная точка поиска\
sAMAccountName=username – искомый пользователь

### Проверка логина внутренними средствами:

Для тестирования логина пользователя username выполните на управляющей машине следующую команду:

```
ovirt-engine-extensions-tool aaa login-user --profile=domain_example.ru --user-name=username
```

### Проверка записей DNS

В случае, если попытка подключение к LDAP-серверу заканчивается ошибкой вида _Cannot resolve principal '_[_user@domain.ru_](mailto:user@domain.ru)_'_, то это может говорить о том, что служба глобального каталога работает на нестандартном порту 3268 вместо 389, для проверки выполните:

```
dig _ldap._tcp.gc._msdcs.nprt.nn SRV
dig _ldap._tcp.nprt.nn SRV
```

Если обе службы работают на одном порту, то для глобального каталога также следует установить порт 3268.
