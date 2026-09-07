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

# Установка на РЕД ОС

## Системные требования <a href="#requirements" id="requirements"></a>

* [Требования к версии ОС и конфигурации машины](../requirements/);
* Требуемое базовое окружение:
  * "Сервер минимальный" - для актуальных версий брокера, начиная с 3.6-6;
  * "Сервер с графическим интерфейсом" или "Рабочая станция с графическим интерфейсом" - для версий брокера 3.6-5 и ниже;
* Отключенная система контроля доступа SELinux.

## Подготовка системы <a href="#preparation" id="preparation"></a>

Отключите SELinux:

```shell-session
# sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
# setenforce 0
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета пакет брокера `hostvm-vdi_<версия>-<номер_сборки>.rpm` и установите его:

```shell-session
# dnf install ./hostvm-vdi-3.6-3.el8.x86_64.rpm
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
При установке версии 3.6-2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:<br>

```shell-session
# /usr/share/hostvm/postinst.sh
```

\
Начиная с версии 3.6-3 данный шаг не требуется.
{% endhint %}

{% hint style="info" %}
Для дальнейшей установки версий 3.6-3 и 3.6-4 запустите скрипт для инициализации брокера:

```shell-session
# /usr/share/hostvm/broker-init.sh
```

Следуйте указаниям установочного скрипта.

Начиная с версии 3.6-5 данный шаг не требуется.
{% endhint %}

