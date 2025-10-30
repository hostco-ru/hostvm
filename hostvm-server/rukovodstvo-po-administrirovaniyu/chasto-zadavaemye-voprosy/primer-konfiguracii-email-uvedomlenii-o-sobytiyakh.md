# Пример конфигурации email уведомлений о событиях

{% hint style="info" %}
Все описанные шаги производятся в ОС HOSTVM Manager
{% endhint %}

{% hint style="info" %}
В данном примере конфигурации используется почтовый сервис smtp.gmail.com
{% endhint %}

## Подготовка HOSTVM Manager для отправки оповещений

Добавление сервиса smtp в исключения firewall:

```
firewall-cmd --permanent --add-service=smtp
```

Установка зависимостей для SASL аутентификации:

```
yum install cyrus-sasl cyrus-sasl-lib cyrus-sasl-plain -y
```

### Настройка Postfix

Отредактируйте файл /etc/postfix/main.cf

```
myhostname = engine455.test
myorigin = $myhostname
relayhost = [smtp.gmail.com]:587

smtp_tls_security_level = may
smtp_tls_loglevel = 1
meta_directory = /etc/postfix
shlib_directory = /usr/lib64/postfix

smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_sasl_mechanism_filter = plain,login
sender_canonical_maps = hash:/etc/postfix/sender_canonical

```

Создайте файл /etc/postfix/sasl\_passwd со следующим содержимым:

```
[smtp.gmail.com]:587    HOSTVM.example@gmail.com:passwdexample 
```

где HOSTVM.example@gmail.com - email адрес с которого будут приходить уведомления;

passwdexample - пароль приложения google.

По умолчанию письма будут приходить от `root@engine455.test`. Для того, чтобы переписать отправителя на gmail адрес cоздайте файл /etc/postfix/sender\_canonical со следующим содержимым:

```
root@engine455.test hostvm.example@gmail.com
```

Измените права доступа на файл sasl\_passwd:

```
chmod 600 /etc/postfix/sasl_passwd
```

&#x20;Создайте индексированные файлы для postfix:

```
postmap /etc/postfix/sasl_passwd
postmap /etc/postfix/sender_canonical
```

Проверьте корректность конфигурации, после чего запустите сервис postfix:

```
postfix check
systemctl enable postfix
systemctl restart postfix
systemctl status postfix
postconf -n | egrep -i 'relayhost|sasl|tls|canonical|origin|hostname'
```

Отправка тестового сообщения:

```
echo "Test mail from HOSTVM" | mail -s "test Subject" your.mail@gmail.com
```

### Настройка ovirt-engine-notifier

Скопируйте файл ovirt-engine-notifier.conf в директорию _/etc/ovirt-engine/notifier/notifier.conf.d_ и переименуйте в _90-email-notify.conf:_

```
# cp /usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf /etc/ovirt-engine/notifier/notifier.conf.d/90-email-notify.conf
```

Отредактируйте файл 90-email-notify.conf:

```
MAIL_SERVER=localhost
MAIL_PORT=25
MAIL_FROM=ovirt-engine@engine455.test
```

{% hint style="info" %}
В файле можно указать получателей, без добавления в БД

Для этого пропишите:\
FILTER="include:_(smtp:ops@example.com)_ includ&#x65;_:_(_smtp:oncall@example.com_) ${FILTER}"

Конструкция include:\*(smtp:email-адрес) — подписывает email-адрес на все события. Можно перечислить несколько таких include подряд — по одному на адрес.
{% endhint %}

Перезагрузите сервисы:

```
systemctl daemon-reload
systemctl enable ovirt-engine-notifier.service
systemctl restart ovirt-engine-notifier.service
```

Создание тестового пользователя для проверки конфигурации:

<pre><code>ovirt-aaa-jdbc-tool user add test-user
ovirt-aaa-jdbc-tool user edit test-user --attribute=email=your.mail@gmail.com
ovirt-aaa-jdbc-tool user password-reset test-user --password-valid-to="2026-08-01 12:00:00-0800"
<strong>ovirt-aaa-jdbc-tool user show test-user
</strong></code></pre>

На портале администрирования добавьте пользователя test-user:

Administration -> Users -> Add -> добавьте  пользователя.

Подпишите пользователя test-user на необходимые события:

* Выберите пользователя, нажмите на **User Name**, чтобы перейти на подробную страницу о пользователе.
* На вкладке **Event Notifier**, нажмите **Manage Events**
* Выберите интересующие события.
* В поле **Mail Recipient** введите адрес почты пользователя.
* Нажмите "Ок".

Для проверки работоспособности уведомлений необходимо вызвать событие или события, на которые подписан пользователь, например, выключить хост виртуалзиации.&#x20;
