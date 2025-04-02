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

# Установка на РЕД ОС

{% hint style="warning" %}
Статья написана для РЕД ОС версии 7.3.4 с отключенной системой контроля доступа SELinux.
{% endhint %}

{% hint style="warning" %}
Требуемое базовое окружение: **Сервер с графическим интерфейсом** или **Рабочая станция** **с графическим интерфейсом**
{% endhint %}

## Подготовка системы <a href="#preparation" id="preparation"></a>

Отключите SELinux:

```bash
sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/selinux/config
sudo setenforce 0
```

Скачайте и разархивируйте пакет из личного кабинета:

```bash
tar -xvf ./hostvm-vdi<номер версии>-<номер сборки>.rpm.tar.bz2
```

## Установка и настройка <a href="#install" id="install"></a>

Установите скачанный пакет:

```bash
sudo yum install ./hostvm-vdi-3.6-3.el8.x86_64.rpm
```

{% hint style="info" %}
При установке версии 3.6-2 и ниже запустите скрипт для установки необходимых зависимостей и настройки системы:\


```bash
sudo /usr/share/hostvm/postinst.sh
```

\
Начиная с версии 3.6-3 данный шаг не требуется.
{% endhint %}

Запустите скрипт для инициализации брокера:

```bash
sudo /usr/share/hostvm/broker-init.sh
```

Следуйте указаниям установочного скрипта.

Для доступа в веб-интерфейс управления брокера и дальнейшей настройки (включая изменение пароля встроенной учетной записи администратора) обратитесь к разделу:

[Доступ к веб-интерфейсу управления](./#accessing-web-interface)

Для настройки SSL сертификатов:

[Настройка SSL сертификатов](./#ssl-certificates)

