# Отключение принудительного перенаправления HTTP на HTTPS

{% hint style="warning" %}
**Внимание!** Использование протокола HTTP снижает уровень безопасности, так как сетевой трафик и учётные данные передаются без шифрования. Включение этой опции допускается только в условиях изолированного тестового контура.
{% endhint %}

По умолчанию брокер использует HTTPS-соединение. Для использования HTTP необходимо отключить перенаправление в брокере и выполнить настройку Nginx.

1. В административной панели управления перейдите в раздел `Инструменты` -> `Конфигурация` -> Выключите чекбокс `redirectToHttps`. Сохраните изменения.

<figure><img src="../../../.gitbook/assets/redirectToHttps.jpg" alt=""><figcaption></figcaption></figure>

2. В консоли брокера HOSTVM VDI выполните команды:

```
unlink /etc/nginx/sites-available/hostvm  
```

```
ln -s /etc/nginx/sites-available/hostvm.insecure /etc/nginx/sites-enabled  
```

3. Перезапустите службы:

```
systemctl restart nginx vdi.service vdiweb.service
```
