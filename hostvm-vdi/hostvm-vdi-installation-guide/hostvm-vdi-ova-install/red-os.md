# Установка на РЕД ОС

## Системные требования <a href="#requirements" id="requirements"></a>

* Операционная система РЕД ОС версии 7.3.4 с отключенной системой контроля доступа SELinux.
* Требуемое базовое окружение: **Сервер с графическим интерфейсом** или **Рабочая станция с графическим интерфейсом**.

## Подготовка системы <a href="#preparation" id="preparation"></a>

Отключите SELinux:

```shell-session
# sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
# setenforce 0
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета и установите пакет брокера:

```shell-session
# yum install ./hostvm-vdi-3.6-3.el8.x86_64.rpm
```

{% hint style="info" %}
При установке версии 3.6-2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:\


```shell-session
# /usr/share/hostvm/postinst.sh
```

\
Начиная с версии 3.6-3 данный шаг не требуется.
{% endhint %}

{% hint style="info" %}
Для дальнейшей установки версии 3.6-4 и ниже запустите скрипт для инициализации брокера:

```shell-session
# /usr/share/hostvm/broker-init.sh
```

Следуйте указаниям установочного скрипта.
{% endhint %}

Для дальнейшей установки версии 3.6-5 и выше запустите мастер настройки брокера командой:

```shell-session
# hostvm-vdi setup
```

Выполните настройку следуя подсказкам мастера.

Для доступа в веб-интерфейс управления брокера и дальнейшей настройки (включая изменение пароля встроенной учетной записи администратора) обратитесь к разделу:

[Доступ к веб-интерфейсу управления](./#accessing-web-interface)

Для настройки SSL сертификатов:

[Настройка SSL сертификатов](./#ssl-certificates)

