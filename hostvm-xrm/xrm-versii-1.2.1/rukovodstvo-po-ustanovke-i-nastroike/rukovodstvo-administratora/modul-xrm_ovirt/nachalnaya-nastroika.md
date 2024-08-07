# Начальная настройка

{% hint style="danger" %}
Р**епликация не заложена в логику работы модуля xrm\_ovirt и выполняется сторонними средствами: аппаратным или программным обеспечением.**&#x20;

**Требования к репликации описаны в разделе:** [**Описание типовой конфигурации HOSTVM VDI для реализации DR**](../modul-xrm\_openuds/opisanie-tipovoi-konfiguracii-hostvm-vdi-dlya-realizacii-dr.md)
{% endhint %}

{% hint style="info" %}
В разделе приведены обязательные параметры для настройки и примеры их заполнения.\
\
Подробное пошаговое описание процесса развертывания доступно в разделе: [Руководство по внедрению в среде HOSTVM](rukovodstvo-po-vnedreniyu-v-srede-hostvm.md)
{% endhint %}

Перейдите во вкладку Модули в раздел xrm\_ovirt.&#x20;

В секции "ПАРАМЕТРЫ" необходимо настроить следующие параметры:



**01\_site\_primary\_url**&#x20;

Адрес API сервера управления (Engine) основной площадки.&#x20;

Пример: [https://engine1.localdomain/ovirt-engine/api](https://engine1.localdomain/ovirt-engine/api)



**02\_site\_primary\_username**

Административный логин для сервера управления (Engine) основной площадки.&#x20;

Пример:&#x20;

admin@internal (для oVirt 4.4.\*)

admin@ovirt@internal (для oVirt 4.5.\* при использовании встроенного Keycloak)



**03\_site\_primary\_password**

Пароль от административного логина.



**04\_site\_secondary\_url,**&#x20;

Адрес API сервера управления (Engine) резервной площадки.&#x20;

Пример: [https://engine2.localdomain/ovirt-engine/api](https://engine2.localdomain/ovirt-engine/api)



**05\_site\_secondary\_username**

Административный логин для сервера управления (Engine) резервной площадки.&#x20;

Пример:&#x20;

admin@internal (для oVirt 4.4.\*)

admin@ovirt@internal (для oVirt 4.5.\* при использовании встроенного Keycloak)



**06\_site\_secondary\_password**

Пароль от административного логина.



**07\_primary\_storage**

Перечень хранилищ (Storage Domain), видимых серверами кластера виртуализации на основной площадке, которые будут использованы в ходе аварийного восстановления.

В случае наличия нескольких хранилищ - они указываются списом через разделитель ; (точка с запятой).

<table><thead><tr><th width="172">Тип хранилища</th><th width="263">Формат управляющего URL</th><th>Пример управляющего URL</th></tr></thead><tbody><tr><td>NFS</td><td>nfs://%IP_ADDRESS%:/%PATH%</td><td>nfs://192.168.0.1:/nfsdata</td></tr><tr><td>iSCSI</td><td>iscsi://%DOMAIN_ID%:/%IP_ADDRESS%: %PORT%:/%IQN%</td><td>iscsi://bcca8438-810f-4932-bf25-d874babd97b1:/192.168.0.1:3260:/iqn.2006-01.com.openfiler:vm-data1</td></tr><tr><td>FC/FCoE</td><td>fcp://%DOMAIN_ID%</td><td>fcp://3dd5d4c2-e9b8-4046-aa98-499d6f91cb19</td></tr></tbody></table>

**Примечание:** в XRM версии 1.2 поддерживаются хранилища NFS, iSCSI, FC/FCoE.



**08\_secondary\_storage**

Перечень реплик хранилищ (Storage Domain), видимых серверами кластера виртуализации на резервной площадке, которые необходимо подключить в ходе аварийного восстановления.

В случае наличия нескольких хранилищ - они указываются списком через разделитель ; (точка с запятой).

Формат адресов реплик хранилищ аналогичен приведенным выше примерам для параметра 07\_primary\_storage.



**09\_additional\_params**

Дополнительные параметры для использования в рамках плана восстановления (необязательно, указываются при необходимости)



{% hint style="info" %}
В разделе приведены обязательные параметры для настройки и примеры их заполнения.



Подробное пошаговое описание процесса развертывания доступно в разделе: [Руководство по внедрению в среде HOSTVM](rukovodstvo-po-vnedreniyu-v-srede-hostvm.md)
{% endhint %}
