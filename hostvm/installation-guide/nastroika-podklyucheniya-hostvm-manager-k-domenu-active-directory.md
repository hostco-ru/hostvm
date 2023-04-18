# Настройка подключения HOSTVM Manager к домену Active Directory

В данном примере приведена конфигурация подключения HOSTVM Manager к домену Active Directory.

Шаблоны конфигурации можно найти в управляющей виртуальной машине в директории /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad.

Файлы профиля AD должны быть расположены в управляющей виртуальной машине в директориях /etc/ovirt-engine/aaa/ и /etc/ovirt-engine/extensions.d/:

В директории /etc/ovirt-engine/aaa/ создайте файл профиля **example.ru.properties** c конфигурацией:

```
include = <ad.properties>
```

{% hint style="info" %}
Примечание:

Если аутентификация занимает продолжительное время, используйте следующий параметр:

include = \<ad-recursive.properties>
{% endhint %}

```
vars.domain = example.ru
vars.user = CN=<Common-Name>,OU=<Organizational Unit>,OU=<Organizational Unit>,DC=example,DC=ru
vars.password = password ///Ваш пароль

pool.default.auth.simple.bindDN = ${global:vars.user}
pool.default.auth.simple.password = ${global:vars.password}
pool.default.serverset.type = srvrecord
pool.default.serverset.srvrecord.domain = ${global:vars.domain}
pool.default.ssl.startTLS = true
pool.default.ssl.insecure = true

# Create keystore, import root certificate and uncomment
# if using ssl/tls.
#pool.default.ssl.startTLS = true
#pool.default.ssl.truststore.file = ${local:_basedir}/${global:vars.domain}.jks
#pool.default.ssl.truststore.password = changeit

# Optional DNS servers, if enterprise
# DNS server cannot resolve the forest srvrecord.
# In most cases domain controllers are also DNS servers,
# if not, find few within forest.
#
#vars.dns = dns://dc1.${global:vars.forest} dns://dc2.${global:vars.forest}

# Create keystore, import root certificate and uncomment
# if using ssl/tls.
#pool.default.ssl.startTLS = true
#pool.default.ssl.truststore.file = ${local:_basedir}/${global:vars.forest}.jks
#pool.default.ssl.truststore.password = changeit

# Active directory scale settings
# Resolve record via a specific active directory site:
#pool.default.serverset.srvrecord.domain-conversion.type = regex
#pool.default.serverset.srvrecord.domain-conversion.regex.pattern = ^(?<domain>.*)$
#pool.default.serverset.srvrecord.domain-conversion.regex.replacement = Default-First-Site-Name._sites.${domain}
# May be overridden per domain, for example for xxx.com:
#pool.default.dc-resolve.xxx_com.serverset.srvrecord.domain-conversion.regex.replacement = Non-Default-First-Site-Name._sites.${domain}

```

Если используется TLS или SSL протокол для подключения к LDAP серверу, загрузите корневой CA сертификат для сервера LDAP и используйте его для создания открытого keystore файла.

Раскомментируйте соответствующий блок в примере выше и укажите полный путь к общедоступному файлу keystore и пароль для доступа к нему.

В директории /etc/ovirt-engine/extensions.d/ создайте файлы параметров **example.ru-authn.properties** и **example.ru-authz.properties**:

Содержимое **example.ru-authn.properties**:

```
ovirt.engine.extension.name = example.ru-authn
ovirt.engine.extension.bindings.method = jbossmodule
ovirt.engine.extension.binding.jbossmodule.module = org.ovirt.engine-extensions.aaa.ldap
ovirt.engine.extension.binding.jbossmodule.class = org.ovirt.engineextensions.aaa.ldap.AuthnExtension
ovirt.engine.extension.provides = org.ovirt.engine.api.extensions.aaa.Authn
ovirt.engine.aaa.authn.profile.name = example.ru
ovirt.engine.aaa.authn.authz.plugin = example.ru-authz
config.profile.file.1 = ../aaa/example.ru.properties
```

Содержимое **example.ru-authz.properties**:

```
ovirt.engine.extension.name = example.ru-authz
ovirt.engine.extension.bindings.method = jbossmodule
ovirt.engine.extension.binding.jbossmodule.module = org.ovirt.engine-extensions.aaa.ldap
ovirt.engine.extension.binding.jbossmodule.class = org.ovirt.engineextensions.aaa.ldap.AuthzExtension
ovirt.engine.extension.provides = org.ovirt.engine.api.extensions.aaa.Authz
config.profile.file.1 = ../aaa/example.ru.properties
```

Убедитесь, что права и разрешения файла конфигурации профиля соответствуют требованиям:

```
# chown ovirt:ovirt /etc/ovirt-engine/aaa/example.ru.properties
# chmod 600 /etc/ovirt-engine/aaa/example.ru.properties
```

Перезагрузите сервис Engine:

```
# systemctl restart ovirt-engine.service
```

После добавления вышеуказанных файлов конфигурации на странице входа в портал администрирования появится доступный для выбора профиль example.ru.

Чтобы предоставить учетным записям пользователей на сервере LDAP соответствующие разрешения, например, для входа в VM Portal, необходимо дополнительно присвоить права пользователю через портал администрирования.
