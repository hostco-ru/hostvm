---
description: >-
  Данное руководство описывает процесс настройки единой аутентификации (Single
  Sign-On) для HOSTVM Manager с использованием протоколов LDAP и Kerberos.
---

# Настройка единого входа (SSO) в HOSTVM Manager через LDAP и Kerberos

{% hint style="warning" %}
Функции единого входа на портал и на виртуальные машины являются взаимоисключающими. При активации входа на портал вход на ВМ блокируется, так как для делегирования учетных данных требуется прямой ввод пароля.
{% endhint %}

### Требования

1. Вход на портал управления должен осуществляться по полному доменному имени (FQDN), содержащему домен, с которым будет настроена связь (пример: `engine.hostvm.test`). При отсутствии домена вход не будет работать.
2. HOSTVM Manager должен обязательно разрешаться по полному имени для пользователей домена.
3. Между контроллером домена и HOSTVM Manager должна быть синхронизирована время.
4. Вход по протоколу Kerberos возможен только с АРМ, введённой в данный домен, и с предварительно настроенного на ней браузера.

### Подготовка контроллера домена для работы с HOSTVM Manager

#### 1. Создание сервисной учетной записи

1. Создайте учетную запись (УЗ) в Active Directory.

```
Пример имени: hostvm
```

Задайте параметры учетной записи:

* Имя - `hostvm`
* Имя входа пользователя - `hostvm`

Настройте параметры пароля:

* Отключите «Требовать смену пароля при следующем входе в систему»
* Включите «Запретить смену пароля пользователем»
* Включите «Срок действия пароля не ограничен»

Откройте свойства учетной записи, перейдите на вкладку «Учетная запись» → раздел «Параметры учетной записи» и включите:

* «Данная учетная запись поддерживает 128-разрядное шифрование»
* «Данная учетная запись поддерживает 256-разрядное шифрование»

На основе этих параметров создаётся keytab-файл. Если параметры не указаны, управляющая машина не сможет расшифровать билеты.

#### 2. Создание keytab-файла для Apache

Выполните команду на контроллере домена (Windows) от имени администратора:

```
ktpass -princ HTTP/engine.hostvm.test@HOSTVM.TEST -mapuser HOSTVM\hostvm -crypto ALL -ptype KRB5_NT_PRINCIPAL -pass Passw0rd -out C:\Temp\http.keytab
```

Где замените на свои значения:

* engine.hostvm.test — полное доменное имя сервера HOSTVM Manager
* @HOSTVM.TEST — домен Kerberos (должен быть в верхнем регистре)
* HOSTVM\hostvm — учётная запись AD, к которой будет привязан principal (домен\имя\_пользователя)
* Passw0rd — пароль учётной записи, указанной в `-mapuser`. Должен совпадать с паролем сервисной УЗ в AD
* C:\Temp\http.keytab — путь для сохранения созданного keytab-файла и его имя

#### 3. Перемещение keytab-файла на управляющую машину в среду виртуализации

Скопируйте созданный keytab-файл, например, с помощью scp:

```
scp http.keytab root@engine.hostvm.test:/etc/httpd
```

### Настройка HOSTVM Manager

1. **Установка прав доступа на файл keytab:**

```
chown apache /etc/httpd/http.keytab
chmod 400 /etc/httpd/http.keytab
```

2. **Установка необходимых пакетов**

```
dnf install -y ovirt-engine-extension-aaa-misc ovirt-engine-extension-aaa-ldap mod_auth_gssapi mod_session
```

3. Копирование конфигурации Apache для SSO

```
cp /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad-sso/aaa/ovirt-sso.conf /etc/httpd/conf.d/ovirt-sso.conf
```

4. Копирование шаблонов конфигурации (с указанием имени домена в названии файла)

Файл конфигурации LDAP

```
cp /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad-sso/aaa/profile1.properties /etc/ovirt-engine/aaa/hostvm.test.properties
```

Файл конфигурации авторизации

```
cp /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad-sso/extensions.d/profile1-authz.properties /etc/ovirt-engine/extensions.d/hostvm.test-authz.properties
```

Файл конфигурации аутентификации

```
cp /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad-sso/extensions.d/profile1-http-authn.properties /etc/ovirt-engine/extensions.d/hostvm.test-http-authn.properties
```

Файл конфигурации проверки подлинности

```
cp /usr/share/ovirt-engine-extension-aaa-ldap/examples/ad-sso/extensions.d/profile1-http-mapping.properties /etc/ovirt-engine/extensions.d/hostvm.test-http-mapping.properties
```

5. **Редактирование файла конфигурации LDAP**

```
nano /etc/ovirt-engine/aaa/hostvm.test.properties
```

Пример параметров (укажите свои значения):

```
vars.forest = hostvm.test
vars.user = hostmv
vars.password = Passw0rd
```

6. **Редактирование файла конфигурации аунтификации**

```
nano /etc/ovirt-engine/extensions.d/hostvm.test-http-authn.properties
```

Приведите параметры к виду:

```
ovirt.engine.extension.name = hostvm.test-http-authn
ovirt.engine.aaa.authn.profile.name = hostvm.test-http
ovirt.engine.aaa.authn.authz.plugin = hostvm.test-authz
ovirt.engine.aaa.authn.mapping.plugin = hostvm.test-http-mapping
```

7. **Редактирование файла конфигурации авторизации**

```
nano /etc/ovirt-engine/extensions.d/hostvm.test-authz.properties
```

Приведите указанные параметры к виду:

<pre><code><strong>ovirt.engine.extension.name = hostvm.test-authz
</strong>config.profile.file.1 = ../aaa/hostvm.test.properties
</code></pre>

8. **Редактирование файла конфигурации проверка подлинности (mapping)**

```
nano /etc/ovirt-engine/extensions.d/hostvm.test-http-mapping.properties
```

Приведите указанный параметр к виду:

```
ovirt.engine.extension.name = hostvm.test-http-mapping
```

9. Установка прав доступа на файлы конфигурации

```
chown ovirt:ovirt /etc/ovirt-engine/aaa/*
chown ovirt:ovirt /etc/ovirt-engine/extensions.d/*
chmod 600 /etc/ovirt-engine/aaa/hostvm.test.properties
chmod 640 /etc/ovirt-engine/extensions.d/hostvm.test*
```

10. **Перезапустите служб**

```
systemctl restart httpd.service
systemctl restart ovirt-engine.service
```

### Предоставление прав для пользователей

После того как управляющая машина успешно подключена к домену, вы можете добавить учётные записи доменных пользователей в систему и назначить им необходимые права.

1. Войдите на портал администратора, например под учетной записью `admin@internal`.
2. Для назначения прав пользователям домена следуйте инструкции «[Назначение ролей пользователям](../../rukovodstvo-po-administrirovaniyu/upravlenie-platformoi/roli-i-polnomochiya-dlya-upravleniya-infrastrukturoi/razresheniya-i-roli/naznachenie-rolei-polzovatelyam.md)»:

* выберите подключённый домен;
* найдите необходимые учётные записи;
* предоставьте им соответствующие права.

### Настройка браузера на клиенте для SSO-аутентификации

Для корректного входа на веб-портал HOSTVM Manager с использованием аутентификации по протоколу Kerberos необходимо выполнить дополнительную настройку браузера на клиентских устройствах.

#### Настройка для браузера Mozilla Firefox

1. Откройте браузер и в адресной строке введите `about:config`.
2. Добавьте полное доменное имя HOSTVM Manager (например, `engine.hostvm.test`) в значения следующих параметров:

```
network.negotiate-auth.trusted-uris
network.automatic-ntlm-auth.trusted-uris
```

Пример:

```
engine.hostvm.test
```

#### Настройка для браузера Microsoft Edge (и Internet Explorer)

1. Нажмите «Пуск», введите «Свойства браузера» и откройте соответствующее окно.
2. Перейдите на вкладку «Безопасность».
3. Выделите зону «Местная интрасеть» и нажмите кнопку «Сайты»
4. Нажмите «Дополнительно» и добавьте в зону используемый домен. Для задания шаблона используйте символ `*`.

```
Пример: *.hostvm.test
```

5. Сохраните настройки, нажимая «ОК» во всех открытых окнах.

#### Настройка для браузера Google Chrome

Запустите браузер из командной строки со следующими параметрами:

```
chrome.exe --auth-server-whitelist="*.hostvm.test" --auth-negotiate-delegate-whitelist="*.hostvm.test"
```

Для постоянного применения можно создать ярлык браузера и в поле «Объект» добавить указанные параметры после пути к исполняемому файлу.

### Настройка защищенного соединения startTLS между HOSTVM Manager и контролером домена

1. Получите корневой сертификат центра сертификации или самоподписанный сертификат контроллера Active Directory.
2. Загрузите сертификат в формате **.crt** на HOSTVM Managerв директорию /root.
3. Импортируйте сертификат в хранилище доверенных корневых ЦС выполнив команду на управляющей машине

```
keytool -importcert -noprompt -trustcacerts -alias hostvm-root-ca -file /root/hostvm_Root_CA.crt -keystore /etc/ovirt-engine/aaa/hostvm-ca.jks -storepass Passw0rd
```

Где замените на свои значения:

* hostvm-root-ca — псевдоним сертификата в хранилище
* /root/hostvm\_Root\_CA.crt — путь и имя скопированного сертификата
* hostvm-ca.jks - имя хранилища ключей
* Passw0rd — пароль для доступа к хранилищу

4. Убедитесь, что SRV-записи успешно определяются

```
dig _ldap._tcp.gc._msdcs.hostvm.test SRV
dig _ldap._tcp.hostvm.test SRV
```

Замените `hostvm.test` на ваше доменное имя.

5. Откройте файл `/etc/ovirt-engine/aaa/hostvm.test.properties` (вместо `hostvm.test` укажите имя вашего домена). Раскомментируйте и приведите к следующему виду строки:

```
pool.default.ssl.startTLS = true
pool.default.ssl.truststore.file = /etc/ovirt-engine/aaa/hostvm-ca.jks
pool.default.ssl.truststore.password = Passw0rd
```

Где замените на свои значения исполлуюемые на шаге 3:

* hostvm-ca.jks — псевдоним сертификата в хранилище
* Passw0rd — пароль для доступа к хранилищу

6. Перезапустите службу

```
systemctl restart ovirt-engine
```

7. Убедитесь, что пользователи из LDAP/AD могут успешно аутентифицироваться в HOSTVM Manager.
