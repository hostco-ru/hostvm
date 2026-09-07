---
layout:
  width: default
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Установка на ALT Linux

## Системные требования <a href="#requirements" id="requirements"></a>

* [Требования к версии ОС и конфигурации машины](../requirements/);
* Профиль установки - "Минимальная установка".

## Подготовка системы <a href="#preparation" id="preparation"></a>

Перед установкой убедитесь в наличии записи сервера DNS в файле /etc/resolv.conf.

Обновите систему:

```shell-session
# apt-get update
# apt-get dist-upgrade
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета пакет брокера `hostvm-vdi-<версия>-<номер_сборки>.rpm` и установите его:

```shell-session
# apt-get install ./hostvm-vdi-3.6-alt3.x86_64.rpm
```

Для продолжения установки брокера необходимо отключить службу apache2, если она используется:

```shell-session
# systemctl disable --now httpd2
```

Запустите мастер настройки брокера командой:

```shell-session
# hostvm-vdi setup
```

Выполните настройку следуя подсказкам мастера.

Для доступа в веб-интерфейс управления брокера и дальнейшей настройки (включая изменение пароля встроенной учетной записи администратора) обратитесь к разделу:

[Доступ к веб-интерфейсу управления](virtual-appliance.md#accessing-web-interface)

Для настройки SSL сертификатов:

[Настройка SSL сертификатов](virtual-appliance.md#ssl-certificates)

### Установка и настройка (предыдущие версии) <a href="#install-previous" id="install-previous"></a>

Вместо мастера настройки брокера используйте следующие команды:

{% hint style="info" %}
При установке версии 3.6-alt2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:<br>

```shell-session
# /usr/share/hostvm/postinst.sh
```

\
Начиная с версии 3.6-alt3 данный шаг не требуется.
{% endhint %}

{% hint style="info" %}
Для дальнейшей установки версий 3.6-alt3 и 3.6-alt4 запустите скрипт для инициализации брокера:

```shell-session
# /usr/share/hostvm/broker-init.sh
```

Следуйте указаниям установочного скрипта.

Начиная с версии 3.6-alt5 данный шаг не требуется.
{% endhint %}

