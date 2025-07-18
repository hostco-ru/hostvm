# Провайдер HOSTVM / РОСА/ zVirt / РЕД Виртуализация

В данном разделе описывается развертывание платформы VDI через виртуальную инфраструктуру HOSTVM / РОСА / zVirt / РЕД Виртуализации.

## Создание сервис-провайдера <a href="#provider" id="provider"></a>

Для регистрации сервис-провайдера HOSTVM/РОСА/zVirt/РЕД Виртуализация необходимо зайти в раздел «Services», нажать «New» и выбрать тип провайдера «HOSTVM/ROSA/zVirt/RED Virtualization Platform Provider».

<figure><img src="../../../.gitbook/assets/hostvm-rosa-zvirt-red platform provider.png" alt=""><figcaption></figcaption></figure>

При создании провайдера должны быть настроены следующие параметры:

**Основные:**

* имя (name) – имя сервисного провайдера;
* хост (host) – имя или IP-адрес менеджера виртуализации;
* имя пользователя (username) – имя пользователя (в формате user@domain), имеющего доступ с правами администратора на менеджер виртуализации;
* пароль (password) – пароль пользователя;

{% hint style="warning" %}
**Если поставщиком единого входа является Keycloak, то имя пользователя, имеющего доступ с правами администратора на менеджер виртуализации, указывается в формате user@ovirt@domain.**
{% endhint %}

<figure><img src="../../../.gitbook/assets/hostvm-rosa-zvirt-red config main.png" alt=""><figcaption></figcaption></figure>

**Дополнительные:**

* количество одновременных задач создания рабочего стола (поле «Creation concurrency»);
* количество одновременных задач удаления рабочего стола (поле «Removal concurrency»);
* тайм-аут при установлении соединения с менеджером виртуализации;
* диапазон MAC-адресов для присвоения виртуальным рабочим столам.

![](../../../.gitbook/assets/vdi_ag7.png)

С помощью кнопки «Test» можно проверить, что соединение выполнено успешно.

После сохранения настроенные сервис-провайдеры будут подтверждены и готовы для создания базовых сервисов в HOSTVM/РОСА/zVirt/РЕД Виртуализации.

Количество сервис-провайдеров типа HOSTVM/РОСА/zVirt/РЕД Виртуализация, регистрируемых в рамках платформы, не ограничено.

Чтобы изменить какой-либо параметр в уже существующих сервис-провайдерах, необходимо выбрать его и нажать «Edit».

<figure><img src="../../../.gitbook/assets/hostvm-rosa-zvirt-red edit.png" alt=""><figcaption></figcaption></figure>

С помощью кнопки «Enter Maintenance Mode» можно приостановить все операции, запущенные платформой для данного сервис-провайдера.

Рекомендуется поставить провайдер в режим обслуживания в случае потери связи или его остановки для обслуживания.

## Создание сервисов <a href="#services" id="services"></a>

### Связанные клоны (Linked Clone) <a href="#linked-clone" id="linked-clone"></a>

После регистрации провайдера HOSTVM/РОСА/zVirt/РЕД Виртуализация, в котором будут созданы рабочие столы, необходимо создать базовые сервисы для генерации тонких клонов ВМ.

Для этого следует открыть сервис-провайдер, в котором будет создан тонкий клон, с помощью двойного щелчка, либо выбором пункта “Detail” в контекстном меню провайдера:

<figure><img src="../../../.gitbook/assets/hostvm-rosa-zvirt-red detail.png" alt=""><figcaption></figcaption></figure>

Нажать «New HOSTVM/ROSA/zVirt/RED Virtualization Linked Clone» для создания нового базового сервиса. Минимальные параметры, которые необходимо настроить.

**Основные параметры:**

* имя (name) – имя сервиса;
* кластер (cluster) – кластер серверов HOSTVM/РОСА/zVirt/РЕД Виртуализация, на котором будут размещены развернутые связанные клоны;
* домен хранилища (datastore domain) – хранилище, установленное для развертывания клонов ВМ;
* зарезервированное место (reserved space) – минимальный порог свободного места в хранилище, для возможности разворачивания клонов.

![](../../../.gitbook/assets/vdi_ag10.png)

**Параметры ВМ:**

* базовая машина (base machine) – шаблон для развертывания виртуальных рабочих столов (golden image);
* память (memory) – объем памяти, который будет присвоен (в мегабайтах);
* гарантированная память (memory guaranteed) – объем памяти, который будет гарантированно доступен для тонких клонов;
* USB – если выбрано, то виртуальные рабочие столы будут поддерживать перенаправление USB-устройств;
* экран/дисплей (display) – протокол подключения административной консоли для виртуальных машин - тонких клонов;
* имена машин (machine names) – префикс имени для всех тонких клонов, которые будут развернуты в этой службе (например, имена машин = win-);
* длина имени (name length) – длина номера суффикса, прикрепленного к корневому имени (например, Name Length = 3, win-000 ... win-999).

![](../../../.gitbook/assets/vdi_ag11.png)

После сохранения этой конфигурации будет готов действующий «HOSTVM/ROSA/zVirt/RED Virtualization Linked Clone» на платформе HOSTVM/РОСА/zVirt/РЕД Виртуализация.

Можно зарегистрировать необходимое количество «HOSTVM/ROSA/zVirt/RED Virtualization Linked Clone» на платформе HOSTVM VDI.

![](../../../.gitbook/assets/vdi_ag12.png)

После настройки всех компонентов среды HOSTVM VDI (сервис-провайдеры, аутентификаторы, менеджеры ОС и транспорты подключений) и создания пула сервисов на сервере HOSTVM/РОСА/zVirt/РЕД Виртуализация Manager можно увидеть развернутые виртуальные рабочие столы на базе тонких клонов HOSTVM/РОСА/zVirt/РЕД Виртуализация.

![](../../../.gitbook/assets/vdi_ag13.png)

### Фиксированные машины (Fixed Machines) <a href="#fixed-machines" id="fixed-machines"></a>

Этот тип базового сервиса позволяет подключать пользователей к существующим виртуальным машинам платформы виртуализации.

Для создания сервиса, в сервис-провайдере перейдите на вкладку `Сервисы`, нажмите `Новый`, выберите из списка тип сервиса `Фиксированные машины`.

**Основные параметры**

_Имя (Name)_: имя сервиса.

_Токен сервиса (Service token)_: если требуется отслеживание брокером "входа" и "выхода" пользователя, запрашивающего данный тип сервиса (чтобы при выходе брокер автоматически освобождал машину, делая ее доступной для подключения другого пользователя), необходимо заполнить поле, задав токен в виде произвольной последовательности букв и цифр. Если поле пустое, брокер будет закреплять машину за пользователем (до ручного сброса администратором системы).

> Если поле `Токен сервиса` заполнено, на машины, заданные в качестве используемых сервисом, необходимо установить соответствующий VDI актор типа `Unmanaged`, и при его настройке задать такое же значение поля `Service Token`, как и в настройках базового сервиса.

**Параметры ВМ**

_Кластер (Cluster)_: кластер платформы виртуализации, на котором размещаются виртуальные машины.

_Машины (Machines)_: виртуальные машины выбранного выше кластера, управление которыми и подключение к ним пользователей будет осуществляться брокером VDI. Отметьте нужные машины в списке.

Нажмите `Сохранить` для завершения настройки сервиса.
