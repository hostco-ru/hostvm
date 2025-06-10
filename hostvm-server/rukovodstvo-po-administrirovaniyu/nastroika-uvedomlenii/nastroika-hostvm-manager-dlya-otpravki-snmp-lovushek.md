# Настройка HOSTVM Manager для отправки SNMP-ловушек

Настройте HOSTVM Manager для отправки SNMP-ловушек одному или нескольким внешним SNMP-менеджерам. SNMP-ловушки содержат информацию о системных событиях; они используются для мониторинга среды HOSTVM. Количество и тип ловушек, отправляемых SNMP-менеджеру, настраиваются на машине HOSTVM Manager.

HOSTVM поддерживает SNMPv2 и SNMPv3. SNMPv3 поддерживает следующие уровни безопасности:

**NoAuthNoPriv** \
SNMP-ловушки отправляются без какой-либо авторизации или обеспечения конфиденциальности.

**AuthNoPriv** \
SNMP-ловушки отправляются с авторизацией по паролю, но без обеспечения конфиденциальности.

**AuthPriv** \
SNMP-ловушки отправляются с авторизацией  по паролю и обеспечением конфиденциальности.

Предварительные требования

* Один или несколько внешних SNMP-менеджеров настроены на прием SNMP-ловушек.
* IP-адреса или FQDN машин, которые будут выполнять функции SNMP-менеджеров. При необходимости, укажите порт, через который HOSTVM Manager получает уведомления. По умолчанию используется UDP-порт 162.
* SNMP Community (только для SNMPv2). Несколько SNMP-менеджеров могут принадлежать к одному community. Системы управления и агенты могут взаимодействовать, только если они находятся в одном community. Community по умолчанию является публичным.
* Trap OID (object identifier) для оповещений. HOSTVM Manager предоставляет OID по умолчанию: 1.3.6.1.4.1.2312.13.1.1. Все типы ловушек отправляются вместе с информацией о событии SNMP-менеджеру, когда этот OID определен. Обратите внимание, что изменение Trap OID по умолчанию не позволяет сгенерированным ловушкам соответствовать информационной базе HOSTVM Manager.
* Имя пользователя SNMP для SNMPv3, уровни безопасности 1, 2 и 3.
* Кодовая фраза SNMP для SNMPv3, уровни безопасности 2 и 3.
* Приватная кодовая фраза SNMP для SNMPv3, уровень безопасности 3.

{% hint style="info" %}
HOSTVM Manager предоставляет MIB (management information bases), расположенные в `/usr/share/doc/ovirt-engine/mibs/OVIRT-MIB.txt` и `/usr/share/doc/ovirt-engine/mibs/REDHAT-MIB.txt`. Прежде чем продолжить, загрузите MIB в свой SNMP-менеджер.
{% endhint %}

Параметры SNMP по умолчанию расположены на машине HOSTVM Manager в файле конфигурации службы уведомлений о событиях /usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf. Для значений, указанных в следующем примере, за основу взяты значения по умолчанию или примеры, приведенные в этом файле. Не редактируйте этот файл, поскольку системные изменения, такие как обновления, могут привести к удалению любых изменений, внесенных в этот файл. Вместо этого скопируйте этот файл в `/etc/ovirt-engine/notifier/notifier.conf.d/<integer>-snmp.conf`, где `<integer>`- это число, указывающее приоритет, с которым файл будет запускаться.

### Процедура&#x20;

1. На машине HOSTVM Manager создайте файл конфигурации SNMP с именем файла `<integer>-snmp.conf`, где `<integer>` - это целое число, указывающее порядок обработки файлов. Например:

```
# vi /etc/ovirt-движок/уведомитель/notifier.conf.d/20-snmp.conf
```

{% hint style="warning" %}
Скопируйте настройки SNMP по умолчанию из файла конфигурации службы уведомлений о событиях `/usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf`. Этот файл содержит комментарии для всех настроек.
{% endhint %}

2. Укажите SNMP-менеджера(ов), SNMP community (только  для SNMPv2) и OID в формате, указанном в следующем примере:

```
SNMP_MANAGERS="manager1.example.com manager2.example.com:162".
SNMP_COMMUNITY=общедоступный 
SNMP_OID=1.3.6.1.4.1.2312.13.1.1 
```

3. Укажите, следует ли использовать SNMP версии 2 (по умолчанию) или 3:

```
SNMP_VERSION=3
```

4. Укажите значение для SNMP\_ENGINE\_ID. Например:

```
SNMP_ENGINE_ID="80:00:00:00:01:02:05:05"
```

5. Для SNMPv3 укажите уровень безопасности для SNMP-ловушек:

Security level 1, NoAuthNoPriv traps:

```
SNMP_USERNAME=NoAuthNoPriv
SNMP_SECURITY_LEVEL=1
```

Security level 2, AuthNoPriv traps, пользователь - **ovirtengine**, с использованием кодовой фразы SNMP Auth **authpass**.

```
SNMP_USERNAME=ovirtengine 
SNMP_AUTH_PROTOCOL=MD5 
SNMP_AUTH_PASSPHRASE=аутентификация 
SNMP_SECURITY_LEVEL=2
```

Security level 3, AuthPriv traps, пользователь - **ovirtengine** с использованием кодовых фраз SNMP Auth **authpass** и  SNMP Priv **privpass**. Например:

```
SNMP_ИМЯ ПОЛЬЗОВАТЕЛЯ=ovirtengine 
SNMP_AUTH_PROTOCOL=MD5 
SNMP_AUTH_PASSPHRASE=аутентификация 
SNMP_PRIVACY_PROTOCOL=AES128 
SNMP_PRIVACY_PASSPHRASE=privpass 
SNMP_SECURITY_LEVEL=3
```

6. Определите, какие события отправлять диспетчеру SNMP:

Примеры событий \
Отправлять все события в профиль SNMP по умолчанию:

```
FILTER="include:*(snmp:) ${FILTER}"
```

Отправлять все события **ERROR** или **ALERT** в профиль SNMP по умолчанию:

```
FILTER="include:*:ERROR(snmp:) ${FILTER}"
```

```
FILTER="include:*:ALERT(snmp:) ${FILTER}"
```

Отправлять события для VDC\_START на указанный адрес электронной почты:

```
FILTER="include:VDC_START(snmp:mail@example.com) ${FILTER}"
```

Отправлять события для всего, кроме VDC\_START, в профиль SNMP по умолчанию:

```
FILTER="exclude:VDC_START include:*(snmp:) ${FILTER}"
```

Фильтр по умолчанию, определённый в ovirt-engine-notifier.conf; если не отключить этот фильтр или не применить переопределяющие фильтры, уведомления отправляться не будут:

```
FILTER="exclude:*"
```

**VDC\_START** - это пример доступных сообщений журнала аудита. Полный список сообщений журнала аудита можно найти в `/usr/share/doc/ovirt-engine/AuditLogMessages.properties`. Также, возможна фильтрация результатов в вашем SNMP-менеджере.

7. Сохраните файл.
8. Запустите службу **ovirt-engine-notifier** и добавьте её в автозагрузку:

```
# systemctl start ovirt-engine-notifier.service
# systemctl enable ovirt-engine-notifier.service
```

Проверьте свой SNMP-менеджер, чтобы убедиться, что он принимает SNMP-ловушки .

{% hint style="info" %}
Должен быть определён один из параметров: **SNMP\_MANAGERS**, **MAIL\_SERVER** или оба в `/usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf` или в созданном вами файле конфигурации, чтобы служба уведомлений могла работать.
{% endhint %}

### Пример файла конфигурации SNMP&#x20;

Для этого примера взяты настройки из файла ovirt-engine-notifier.conf. Файл конфигурации SNMP переопределяет настройки в ovirt-engine-notifier.conf.

{% hint style="warning" %}
Скопируйте настройки SNMP по умолчанию из файла конфигурации службы уведомлений о событиях `/usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf` в `/etc/ovirt-engine/notifier/notifier.conf.d/<`_`integer`_`>-snmp.conf`, где `<`_`integer`_`>` - это число, указывающее порядок обработки файлов. Этот файл содержит комментарии для всех настроек.
{% endhint %}

`/etc/ovirt-engine/notifier/notifier.conf.d/20-snmp.conf`&#x20;

```
SNMP_MANAGERS="manager1.example.com manager2.example.com:162" 1
SNMP_COMMUNITY=public                                         2
SNMP_OID=1.3.6.1.4.1.2312.13.1.1                              3
FILTER="include:*(snmp:)"                                     4
SNMP_VERSION=3                                                5
SNMP_ENGINE_ID="80:00:00:00:01:02:05:05"                      6
SNMP_USERNAME=<username>                                      7
SNMP_AUTH_PROTOCOL=MD5                                        8
SNMP_AUTH_PASSPHRASE=<authpass>                               9
SNMP_PRIVACY_PROTOCOL=AES128                                  10
SNMP_PRIVACY_PASSPHRASE=<privpass>                            11
SNMP_SECURITY_LEVEL=3 Some code                               12
```

1 IP-адреса или FQDN машин, которые будут выполнять функции SNMP-менеджеров. Записи должны быть разделены пробелом и могут содержать номер порта. Например, **manager1.example.com** **manager2.example.com:164**&#x20;

2 (Только для SNMPv2) SNMP Community String по умолчанию. \
SNMP Trap OID для исходящих уведомлений. iso(1) org(3) dod(6) internet(1) private(4) enterprises(1) redhat(2312) ovirt(13) engine(1) notifier(1)

{% hint style="danger" %}
3 Изменение значения по умолчанию приведет к тому, что сгенерированные ловушки не будут соответствовать требованиям OVIRT-MIB.txt.
{% endhint %}

4 Алгоритм, используемый для определения триггеров и получателей SNMP-уведомлений.&#x20;

5 Версия SNMP. Поддерживаются ловушки SNMPv2 и SNMPv3. 2 = SNMPv2, 3 = SNMPv3.&#x20;

6 (Только для SNMPv3) Engine ID, используемый для SNMP-ловушек.&#x20;

7 (Только для SNMPv3) Имя пользователя, используемое для SNMP-ловушек.&#x20;

8 (Только для SNMPv3) Протокол аутентификации SNMP. Поддерживаемые значения - MD5 и SHA. Требуется, если для **SNMP\_SECURITY\_LEVEL** установлено значение 2 (**AUTH\_NOPRIV**) или 3 (**AUTH\_PRIV**).&#x20;

9 (Только для SNMPv3) Ключевая фраза SNMP Auth. Требуется, если для **SNMP\_SECURITY\_LEVEL** установлено значение 2 (**AUTH\_NOPRIV)** или 3 (**AUTH\_PRIV**).&#x20;

10 (Только для SNMPv3) SNMP privacy protocol. Поддерживаемые значения - AES128, AES192 и AES256. Заметьте, что AES192 и AES256 не определены в RFC3826, поэтому убедитесь, что ваш SNMP-сервер поддерживает эти протоколы, прежде чем включать их. Требуется, если для **SNMP\_SECURITY\_LEVEL** установлено значение 3 (**AUTH\_PRIV**).&#x20;

11 (Только для SNMPv3) Ключевая фраза SNMP, используемая, когда для **SNMP\_SECURITY\_LEVEL** задано значение 3 (**AUTH\_PRIV**).&#x20;

12 (Только для SNMPv3) Уровень безопасности SNMP. 1 = **NOAUTH\_NOPRIV**, 2 = **AUTH\_NOPRIV**, 3 = **AUTH\_PRIV**.
