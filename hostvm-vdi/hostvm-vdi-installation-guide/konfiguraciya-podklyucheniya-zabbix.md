# Конфигурация подключения Zabbix

Для интеграции Zabbix с HOSTVM VDI воспользуйтесь готовым шаблоном и скриптами.

Архив доступен для скачивания в личном кабинете.

{% hint style="info" %}
Примечание: необходима версия Zabbix 6 и выше
{% endhint %}

#### Настройка Zabbix-сервера

1\. Установите базовую конфигурацию Zabbix-сервера с помощью [инструкции](https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=centos\&os\_version=8\&components=server\_frontend\_agent\&db=mysql\&ws=nginx) с сайта разработчика.

2\. Авторизуйтесь через браузер по указанному в процессе установки адресу, используя Admin/zabbix в качестве логина/пароля

3\. Загрузите в Zabbix шаблон из ЛК hostvm\_vdi\_zabbix\_template в раздел Configuration -> Templates -> Import

4\. Добавьте хост в разделе Configuration -> Hosts -> Create Host.\
При добавлении укажите имя хоста, адрес Zabbix-агента и присоедините импортированный шаблон.

#### Настройка Zabbix-агента

1\. Установите базовую конфигурацию Zabbix-агента на HOSTVM VDI Broker с помощью [инструкции](https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=debian\&os\_version=11\&components=agent\&db=\&ws=) с сайта разработчика.

2\. Загрузите архив на машину брокера и распакуйте:

```
tar xvzf hostvm_vdi_zabbix_template.tar.gz 
```

3\. Создайте директорию scripts:

```
mkdir -p /etc/zabbix/scripts
```

4\. Скопируйте файл `zbx-hostvm_vdi.conf` в директорию `/etc/zabbix/zabbix_agentd.d/`:

```
cp hostvm_vdi_zabbix_tepmplate/zbx-hostvm_vdi.conf /etc/zabbix/zbx-hostvm_vdi.conf
```

5\. Скопируйте файл `zbx-hostvm_vdi.py` в директорию `/etc/zabbix/scripts/`:

```
cp hostvm_vdi_zabbix_tepmplate/zbx-hostvm_vdi.py /etc/zabbix/scripts/zbx-hostvm_vdi.py
```

6\. Скопируйте директорию `apiclient` в директорию `/etc/zabbix/scripts/`:

```
cp -r hostvm_vdi_zabbix_tepmplate/apiclient /etc/zabbix/scripts/apiclient
```

7\. Установите необходимые права для файла `zbx-hostvm_vdi.py`:

```
chmod 0755 /etc/zabbix/scripts/zbx-hostvm_vdi.py
```

\
8\. Если на Брокере запущен фаервол, то необходимо разрешить подключение по порту 10050/tcp (либо другому, указанному в конфигурации агента). Пример для ufw:

```
sudo ufw allow 10050/tcp
```

9\. Откройте файл `/etc/zabbix/zabbix_agentd.conf` и внесите изменения, пример изменяемых параметров:

```
Server = 10.1.99.30
# IP-адрес Вашего Zabbix-сервера
ServerActive = 10.1.99.30
# IP-адрес Вашего Zabbix-сервера
ListenPort = 10050
# Порт, указанный на Zabbix-сервере при добавлении хоста, в случае, если порт другой, то необходимо добавить для него соответствующее правило фаервола (см. пункт 7)
Hostname = vdi.pvhostvm.ru
# FQDN Вашего хоста, должен быть аналогичен указанному на Zabbix-сервере
Timeout=30
Include=/etc/zabbix/zabbix_agentd.d/zbx-hostvm_vdi.conf 
# В случае, если путь до конфигурационного файла другой, то его нужно изменить
```

10\. Откройте файл `/etc/zabbix/scripts/zbx-hostvm_vdi.py` и внесите изменения, указав параметры Вашего брокера:

```
vdi = apiclient.Client(  # Параметры подключения
    '10.1.99.58',  # адрес брокера
    'admin',  # аутентификатор
    'root',  # пользователь
    'udsmam0'  # пароль
)
```

11\. Перезагрузите агент:

```
systemctl restart zabbix-agent
```

После выполненных действий дождитесь появления объектов в панели управления Zabbix.

\
