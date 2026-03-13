---
description: >-
  В данной статье описаны процедуры включения и отключения расширенного (DEBUG)
  логирования в HOSTVM Manager для диагностики проблем.
---

# Настройка DEBUG-логирования в HOSTVM Manager

{% hint style="warning" %}
Уровень DEBUG записывает огромное количество информации. Длительное использование может привести к быстрому заполнению раздела `/var/log` и падению производительности сервиса. Включайте его только на время диагностики и сразу отключайте.
{% endhint %}

### Общее логирование HOSTVM Manager

#### Включение DEBUG-логирования для всей системы

Для диагностики общих проблем выполните следующие шаги:

1. Подключитесь по SSH к серверу HOSTVM Manager.
2. Включите DEBUG-логирование для всего HOSTVM Manager:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt:write-attribute(name=level,value=DEBUG)"
```

3. Для подтверждения введите пароль от учетной записи admin и примените изменения, перезапустив службу:

```
systemctl restart ovirt-engine.service
```

5. Для отслеживания логов в реальном времени выполните попытку входа проблемного пользователя и наблюдайте за выводом:

```
/tail -f /var/log/ovirt-engine/engine.log | grep -i "username\|ldap\|auth"
```

_Примечание: замените "username" на логин пользователя или первые символы для фильтрации_

#### Отключение DEBUG-логирования

После завершения диагностики обязательно верните уровень логирования обратно на INFO, чтобы избежать излишней нагрузки на дисковую подсистему:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt:write-attribute(name=level,value=INFO)"
```

Для подтверждения введите пароль от учетной записи admin и примените изменения, перезапустив службу:

```
systemctl restart ovirt-engine.service
```

### Логирование отдельных модулей

Для минимизации объема логов и фокусировки на проблемах доступа рекомендуется включать DEBUG только для конкретных пакетов, например отвечающих за аутентификацию (AAA - Authentication, Authorization, Accounting).

**Пакет org.ovirt.engine.core.aaa отвечает за базовые механизмы аутентификации:**

Включение логирования:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt.engine.core.aaa:write-attribute(name=level,value=DEBUG)"
systemctl restart ovirt-engine.service
```

Отключение логирования:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt.engine.core.aaa:write-attribute(name=level,value=INFO)"
systemctl restart ovirt-engine.service
```

**Пакет org.ovirt.engine.core.bll.aaa отвечает за расширеные механизмы аутентификации:**

Включение логирования:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt.engine.core.bll.aaa:write-attribute(name=level,value=DEBUG)"
systemctl restart ovirt-engine.service
```

Отключение логирования:

```
/usr/share/ovirt-engine-wildfly/bin/jboss-cli.sh --connect --controller=localhost:8706 --user=admin@internal --commands="/subsystem=logging/logger=org.ovirt.engine.core.bll.aaa:write-attribute(name=level,value=INFO)"
systemctl restart ovirt-engine.service
```
