# Начальная настройка

{% hint style="info" %}
В разделе приведены обязательные параметры для настройки и примеры их заполнения.\
\
Подробное пошаговое описание процесса развертывания доступно в разделе: [Руководство по внедрению в среде HOSTVM VDI](rukovodstvo-po-vnedreniyu-v-srede-hostvm-vdi.md)
{% endhint %}

Перейдите во вкладку Модули в раздел xrm\_OpenUDS.&#x20;

В секции "ПАРАМЕТРЫ" необходимо настроить следующие параметры:



**01\_broker\_primary\_url**&#x20;

IP-адрес/FQDN основного брокера.

Пример: vdi-primary.pvhostvm.ru



**02\_broker\_primary\_username**

Учётная запись с основного брокера.

Пример: root



**03\_broker\_primary\_authenticator**

Аутентификатор, используемый учётной записи с основного брокера.

Пример: admin



**04\_broker\_primary\_password**&#x20;

Пароль от учётной записи с основного брокера.

Пример: udsmam0



**05\_broker\_secondary\_ip**&#x20;

IP-адрес/FQDN резервного брокера.

Пример: vdi-secondary.pvhostvm.ru



**06\_broker\_secondary\_username**

Учётная запись с резервного брокера.

Пример: root



**07\_broker\_secondary\_authenticator**

Аутентификатор, используемый учётной записи с основного брокера.

Пример: admin



**08\_broker\_secondary\_password**

Пароль от учётной записи с резервного брокера.

Пример: udsmam0



**09\_service\_pool\_name**

Имена сервис-пулов, которые необходимо восстановить, указываются через точку с запятой.

Пример: Windows Static Multiple;Linux Static Multiple



{% hint style="info" %}
В разделе приведены обязательные параметры для настройки и примеры их заполнения.



Подробное пошаговое описание процесса развертывания доступно в разделе: [Руководство по внедрению в среде HOSTVM VDI](rukovodstvo-po-vnedreniyu-v-srede-hostvm-vdi.md)
{% endhint %}
