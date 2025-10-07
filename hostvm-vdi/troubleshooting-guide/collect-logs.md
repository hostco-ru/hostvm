# Сбор диагностической информации

С помощью консольной утилиты `hostvm-vdi` вы можете сгенерировать архив с логами системы для дальнейшей передачи в техническую поддержку.

Для этого выполните команду `hostvm-vdi support create` в консоли брокера:

```shell-session
# hostvm-vdi support create
Support bundle saved to /tmp/hostvm-vdi-broker-logs-20251001_102815.zip
```

Или туннелера:

```shell-session
# hostvm-vdi support create
Support bundle saved to /tmp/hostvm-vdi-gw-logs-20251001_102857.zip
```

В результате выполнения будет сгенерирован архив в директории `/tmp`, полный путь к файлу указывается в выводе команды.

