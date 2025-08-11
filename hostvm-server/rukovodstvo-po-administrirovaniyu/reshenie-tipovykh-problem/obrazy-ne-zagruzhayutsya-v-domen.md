---
description: >-
  Данная статья описывает решение проблемы с загрузкой образов через
  веб-интерфейс
---

# Образы не загружаются в домен

## Проблема

В домен не загружается ISO-образ со следующей ошибкой:\
"Connection to ovirt-imageio service has failed. Ensure that ovirt-engine certificate is registered as a valid CA in the browser."

## Решение

### Способ 1

Установите сертификат HOSTVM Manager в доверенные центры сертификации на машине, с которой производится загрузка образа.

### Способ 2

Подключитесь к управляющей машине и выполните следующие команды:

```
engine-config -s ImageTransferProxyEnabled=false
systemctl restart ovirt-engine
```

## Причина возникновения

На клиентской машине не установлен сертификат управляющей машины.
