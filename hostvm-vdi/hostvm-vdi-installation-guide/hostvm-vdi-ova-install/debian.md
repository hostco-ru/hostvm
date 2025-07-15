# Установка на Debian

## Системные требования <a href="#requirements" id="requirements"></a>

* Операционная система Debian 11 (Bullseye)

## Подготовка системы <a href="#preparation" id="preparation"></a>

Обновите систему:

```shell-session
# apt-get update
# apt-get upgrade
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета пакет брокера `hostvm-vdi_3.6-<номер сборки>.deb` и установите его:

```shell-session
# apt install /tmp/hostvm-vdi_3.6-20250314.deb
```

Запустите мастер настройки брокера командой:

```shell-session
# hostvm-vdi setup
```

Выполните настройку следуя подсказкам мастера.

Для доступа в веб-интерфейс управления брокера и дальнейшей настройки (включая изменение пароля встроенной учетной записи администратора) обратитесь к разделу:

[Доступ к веб-интерфейсу управления](./#accessing-web-interface)

Для настройки SSL сертификатов:

[Настройка SSL сертификатов](./#ssl-certificates)

