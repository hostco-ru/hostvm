---
layout:
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
---

# Установка на ALT Linux

> Данная статья написана для ALT Linux версии 10.02 и выше

## Подготовка системы <a href="#preparation" id="preparation"></a>

Перед установкой убедитесь в наличии записи сервера DNS в файле /etc/resolv.conf.

Обновите систему:

```bash
sudo apt-get update
sudo apt-get dist-upgrade
```

## Установка и настройка <a href="#install" id="install"></a>

Скачайте из личного кабинета и установите пакет брокера:

```bash
sudo apt-get install ./hostvm-vdi-3.6-alt3.x86_64.rpm
```

Для продолжения установки брокера необходимо отключить службу apache2, если она используется:

```bash
sudo systemctl disable --now httpd2
```

{% hint style="info" %}
При установке версии 3.6-alt2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:\


```bash
sudo /usr/share/hostvm/postinst.sh
```

\
Начиная с версии 3.6-3 данный шаг не требуется.
{% endhint %}

Запустите скрипт для инициализации брокера:

```
sudo /usr/share/hostvm/broker-init.sh
```

Следуйте указаниям установочного скрипта.

Для доступа в веб-интерфейс управления брокера и дальнейшей настройки (включая изменение пароля встроенной учетной записи администратора) обратитесь к разделу:

[Доступ к веб-интерфейсу управления](./#accessing-web-interface)

Для настройки SSL сертификатов:

[Настройка SSL сертификатов](./#ssl-certificates)

