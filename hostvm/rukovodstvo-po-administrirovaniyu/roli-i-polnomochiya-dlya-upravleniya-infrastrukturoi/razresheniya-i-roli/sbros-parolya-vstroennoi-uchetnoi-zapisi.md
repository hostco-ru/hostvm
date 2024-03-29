# Сброс пароля встроенной учетной записи

Встроенная учетная запись администратора создается во время установки как пользователь по умолчанию, которой присвоена общесистемная роль SuperUser. Как и root в системе Сentos, она может быть полезна в качестве аварийной учетной записи администрирования, если ваша внешняя служба каталогов не работает. Время от времени вам может понадобиться изменить или сбросить пароль для этой учетной записи. Это можно сделать с помощью команды ovirt-aaa-jdbc-tool. Перезапуск сервисов для вступления изменений в силу не требуется. Чтобы изменить пароль для учетной записи admin@internal, выполните следующую процедуру:

1. Подключитесь по SSH к машине HOSTVM Manager;
2. Чтобы изменить пароль, выполните команду ovirt-aaa-jdbc-tool. Используя параметр user password-reset, укажите имя пользователя. Установите время действия пароля с помощью опции --password-valid-to=2020-08-01 12:00:000". Если вы не укажете время действия, то срок действия пароля будет установлен на текущее время.

```
# ovirt-aaa-jdbc-tool user password-reset admin --password-valid-to="2025-08-01 12:00:00Z"
Password: new_password
Reenter password: new_password
updating user admin...
user updated successfully
```

Учетные записи пользователей во внутреннем локальном домене следуют следующим политикам паролей по умолчанию:

* пароли должны состоять минимум из шести символов;
* последние три пароля не могут быть использованы повторно.

Можно перечислить доступные политики или изменить политику по умолчанию, запустив команду ovirt-aa-jdbc-tool с параметром settings.

Если вы слишком часто предпринимаете попытки войти в HOSTVM Manager под учетной записью admin с неправильным паролем, учетная запись может быть заблокирована. Можно разблокировать учетную запись от имени root на машине HOSTVM Manager, выполнив команду:

```
# ovirt-aaa-jbdc-tool user unlock admin
update user admin... user updated successfully
```
