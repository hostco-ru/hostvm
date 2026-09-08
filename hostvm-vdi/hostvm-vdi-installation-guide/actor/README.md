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

# Агент HOSTVM VDI

Агент HOSTVM VDI устанавливается:

* в шаблоны операционных систем Windows или Linux (базовый (“золотой”) образ) для управления публикацией виртуальных рабочих столов;
* на терминальные серверы RDS и Static IP машины для управления сессиями пользователей.

## Системные требования <a href="#requirements" id="requirements"></a>

### 4.0

* Требуемая версия Python для установки на Linux-based ОС - 3.9 и выше.

### 3.6

* Требуемая версия Python для установки на Linux-based ОС - 3.6 и выше.

## Установка <a href="#install" id="install"></a>

> **Изменения в версии 4.0:** дистрибутивы агента для поддерживаемых ОС больше не распространяются в составе пакета брокера hostvm-vdi, а выделены в отдельный пакет hostvm-vdi-actors для установки (за исключением готовых образов виртуальных машин).

Дистрибутивы агента HOSTVM VDI доступны для загрузки в веб-интерфейсе брокера HOSTVM VDI из-под учетной записи с правами администратора.

Выберите **«Загрузки»** в меню пользователя:

<figure><img src="https://raw.githubusercontent.com/hostco-ru/hostvm/master/.gitbook/assets/actor-1.png" alt=""><figcaption></figcaption></figure>

Выберите дистрибутив, соответствующий типу и версии операционной системы в базовом образе или на терминальном сервере, используемых для предоставления сервисов VDI:

* `udsactor_<версия>_all.deb`: агент для базового образа машин Linux на основе Debian, а также Ubuntu, Xubuntu и т.д. (см. [требования к версии python](./#requirements));
* `udsactor-<версия>-1.noarch.rpm`: агент для базового образа машин Linux на основе Red Hat, а также РЕД ОС, CentOS, Fedora, Suse и т.д. (см. [требования к версии python](./#requirements));
* `udsactor-<версия>-alt1.noarch.rpm`: агент для базового образа машин Linux на основе ALT Linux (см. [требования к версии python](./#requirements));
* `udsactor-unmanaged_<версия>_all.deb`: агент для управления сеансами машин на основе Debian, а также Ubuntu, Xubuntu и т.д., подключенных через сервис-провайдер «Static IP Machines Provider» (см. [требования к версии python](./#requirements));
* `udsactor-unmanaged-<версия>-1.noarch.rpm`: агент для управления сеансами машин на основе Red Hat, а также РЕД ОС, CentOS, Fedora, Suse и т.д. (см. [требования к версии python](./#requirements));
* `udsactor-unmanaged-<версия>-alt1.noarch.rpm`: агент для управления сеансами машин на основе ALT Linux (см. [требования к версии python](./#requirements));
* `udsactor-rds_<версия>_all.deb`: агент для машин Linux на основе Debian, а также Ubuntu, Xubuntu и т.д. (см. [требования к версии python](./#requirements)), предоставляющих доступ к удаленным рабочим столам и приложениям. Используется только с сервис-провайдером RDS;
* `udsactor-rds-<версия>-1.noarch.rpm`: агент для машин Linux на основе Red Hat, а также РЕД ОС, CentOS, Fedora, Suse и т.д. (см. [требования к версии python](./#requirements)), предоставляющих доступ к удаленным рабочим столам и приложениям. Используется только с сервис-провайдером RDS;
* `udsactor-rds-<версия>-alt1.noarch.rpm`: агент для машин Linux на основе ALT Linux (см. [требования к версии python](./#requirements)), предоставляющих доступ к удаленным рабочим столам и приложениям. Используется только с сервис-провайдером RDS;
* `UDSActorSetup-<версия>.exe`: агент для базового образа машин Windows;
* `UDSActorUnmanagedSetup-<версия>.exe`: агент для управления сеансами машин Windows, подключенных через сервис-провайдер «Static IP Machines Provider»;
* `RDSActorSetup-<версия>.exe`: агент для машин Windows Server, предоставляющих доступ к удаленным рабочим столам и приложениям. Используется только с сервис-провайдером RDS.

### Legacy версии агента <a href="#legacy" id="legacy"></a>

> Доступны до версии брокера 3.6 включительно. Начиная с версии 4.0 удалена поддержка legacy агента версии 2.2, дистрибутивы исключены из состава выпуска.

* `udsactor-2.2.0_legacy.deb`: legacy версия агента для базового образа машин Linux на основе Debian, а также Ubuntu, Xubuntu и т.д., где невозможно использовать Python версии 3 (требует python версии 2.7);
* `udsactor-legacy-2.2.1-1.noarch.rpm`: legacy версия агента для базового образа машин Linux на основе Red Hat, а также CentOS, Fedora и т.д., где невозможно использовать Python версии 3 (требует python версии 2.7);
* `udsactor-opensuse-legacy-2.2.1-1.noarch.rpm`: legacy версия агента для базового образа машин Linux на основе OpenSuse, где невозможно использовать Python версии 3 (требует python версии 2.7).

<figure><img src="https://raw.githubusercontent.com/hostco-ru/hostvm/master/.gitbook/assets/download-all-actors.png" alt=""><figcaption></figcaption></figure>
