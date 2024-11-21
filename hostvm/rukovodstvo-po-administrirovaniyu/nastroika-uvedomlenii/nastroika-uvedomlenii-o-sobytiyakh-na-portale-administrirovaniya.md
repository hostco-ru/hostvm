# Настройка уведомлений о событиях на портале администрирования

HOSTVM Manager может уведомлять назначенных пользователей по электронной почте, когда в среде, управляемой HOSTVM Manager, происходят определенные события. Чтобы использовать этот функционал, необходимо настроить MTA (Mail Transfer Agent) для доставки сообщений. Через портал администрирования можно настроить только уведомления по электронной почте. SNMP ловушки должны быть настроены на ВМ HOSTVM Manager.

### Процедура&#x20;

1. Убедитесь, что у вас есть доступ к почтовому серверу, который может автоматически принимать сообщения от HOSTVM Manager и отправлять их членам рассылки.
2. Перейдите в раздел **Administration > Users** и выберите пользователя.
3. Кликните по имени пользователя, чтобы перейти на страницу сведений.
4. На вкладке Event Notifer нажмите **Manage Events**.
5. Нажмите **Expend All** или отдельные контекстные меню для просмотра событий.
6. Установите соответствующие чек-боксы.
7. Введите адрес электронной почты в поле **Mail Recipient**.
8. Нажмите **OK**.
9. На ВМ HOSTVM Manager скопируйте  содержимое файла ovirt-engine-notifier.conf в новый файл с именем **90-email-notify.conf**

```
# cp /usr/share/ovirt-engine/services/ovirt-engine-notifier/ovirt-engine-notifier.conf /etc/ovirt-engine/notifier/notifier.conf.d/90-email-notify.conf
```

10. Отредактируйте файл **90-email-notify.conf**, удалив все, кроме раздела **EMAIL Notifications**
11. Введите корректные значения переменных электронной почты, как в примере ниже. Этот файл переопределяет значения в исходном файле ovirt-engine-notifier.conf.

```
#---------------------#
# EMAIL Notifications #
#---------------------#

# The SMTP mail server address. Required.
MAIL_SERVER=myemailserver.example.com

# The SMTP port (usually 25 for plain SMTP, 465 for SMTP with SSL, 587 for SMTP with TLS)
MAIL_PORT=25

# Required if SSL or TLS enabled to authenticate the user. Used also to specify 'from' user address if mail server
# supports, when MAIL_FROM is not set. Address is in RFC822 format
MAIL_USER=

# Required to authenticate the user if mail server requires authentication or if SSL or TLS is enabled
SENSITIVE_KEYS="${SENSITIVE_KEYS},MAIL_PASSWORD"
MAIL_PASSWORD=

# Indicates type of encryption (none, ssl or tls) should be used to communicate with mail server.
MAIL_SMTP_ENCRYPTION=none

# If set to true, sends a message in HTML format.
HTML_MESSAGE_FORMAT=false

# Specifies 'from' address on sent mail in RFC822 format, if supported by mail server.
MAIL_FROM=rhevm2017@example.com

# Specifies 'reply-to' address on sent mail in RFC822 format.
MAIL_REPLY_TO=

# Interval to send smtp messages per # of IDLE_INTERVAL
MAIL_SEND_INTERVAL=1

# Amount of times to attempt sending an email before failing.
MAIL_RETRIES=4
```

{% hint style="info" %}
Полный список дополнительных опций представлен в файле: **`/etc/ovirt-engine/notifier/notifier.conf.d/README`**&#x20;
{% endhint %}

12. Включите и перезапустите службу ovirt-engine-notifier, чтобы активировать внесенные изменения:

```
# systemctl daemon-reload
# systemctl enable ovirt-engine-notifier.service
# systemctl restart ovirt-engine-notifier.service
```

Теперь выбранный пользователь будет получать электронные письма, основанные на событиях в среде HOSTVM. Выбранные события отображаются на вкладке **Event Notifier** для этого пользователя.
