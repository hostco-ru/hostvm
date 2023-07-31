# Как перегенерировать SSL-сертификаты на Hosted Engine и хостах?

## Обновление сертификатов Engine:

Подключитесь к консоли виртуальной машины Engine и выполните следующие команды:

```
# hosted-engine --set-maintenance --mode=global
# engine-setup --offline
```

Ответьте на вопросы установщика, на вопрос _Renew certificates?_ ответьте Yes.

Выведите Engine из Maintenance:

```
# hosted-engine --set-maintenance --mode=none
```

## Обновление сертификатов хостов через панель администрирования:

Обновление сертификатов хостов:

1. На портале администрирования, нажмите Compute Hosts.
2. Выберите Management Maintenance и нажмите OK. Виртуальные машины должны будут автоматически смигрировать с хоста, или их будет необходимо отключить вручную.
3. Когда на хостах не будет больше активных ВМ, нажмите Installation => Enroll Certificate => Management => Activate



