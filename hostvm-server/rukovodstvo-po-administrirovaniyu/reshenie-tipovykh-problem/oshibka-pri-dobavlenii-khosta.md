# Ошибка при добавлении хоста

В случае возникновения следующей ошибки при добавлении хоста:

> Error while executing action: Cannot add Host. Host with the same UUID already exists.

Выполните команду ниже на добавляемом хосте:

```
uuidgen > /etc/vdsm/vdsm.id
```

После чего снова попробуйте добавить хост.
