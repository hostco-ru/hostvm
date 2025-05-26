---
description: >-
  В данном разделе описывается способ подключения и настройки сервис-провайдера
  OpenGnsys.
---

# Провайдер OpenGnsys

## Описание <a href="#description" id="description"></a>

OpenGnsys — это решение с открытым исходным кодом для централизованной установки, клонирования и управления операционными системами на физических и виртуальных компьютерах.

## Создание сервис-провайдера <a href="#provider" id="provider"></a>

Для создания сервис-провайдера OpenGnsys перейдите в раздел "Сервисы", нажмите "Новый", выберите тип провайдера "OpenGnsys".

<figure><img src="../../../.gitbook/assets/Провайдер OpenGnsys.png" alt=""><figcaption></figcaption></figure>

### **Основные настройки** <a href="#main" id="main"></a>

**Имя (Name)** — наименование создаваемого провайдера для отображения в системе.

**Хост (Host)** — FQDN или IP-адрес хоста OpenGnsys.

**Порт (Port)** — порт подключения к хосту OpenGnsys.

**Использовать SSL (Use SSL)** — проверка SSL сертификата сервера OpenGnsys.

**Имя пользователя (Username)** — имя пользователя с административными правами в OpenGnsys.

**Пароль (Password)** — пароль пользователя.

<figure><img src="../../../.gitbook/assets/Конфигурация Основной.png" alt=""><figcaption></figcaption></figure>

### Вкладка "Параметры" <a href="#parameters" id="parameters"></a>

**Ссылка на UDS сервер (URL)** - URL, используемый OpenGnsys для доступа к брокеру HOSTVM VDI.

<figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

### Расширенные настройки <a href="#advanced" id="advanced"></a>

* **Одновременное создание (Creation concurrency)** — количество одновременных задач создания рабочих столов.
* **Одновременное удаление (Removal concurrency)** — количество одновременных задач удаления рабочих столов.
* **Таймаут (Timeout)** — время ожидания подключения к OpenGnsys (в секундах).

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

