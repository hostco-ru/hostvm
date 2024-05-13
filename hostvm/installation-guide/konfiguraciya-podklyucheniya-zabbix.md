# Конфигурация подключения Zabbix

Для интеграции Zabbix с HOSTVM воспользуйтесь готовым шаблоном.

Архив доступен для скачивания в личном кабинете.

{% hint style="info" %}
Примечание: необходима версия Zabbix 6 и выше
{% endhint %}

### Настройка Zabbix-сервера

1\. Установите базовую конфигурацию Zabbix-сервера с помощью инструкции с сайта разработчика:[ ](https://www.zabbix.com/ru/download?zabbix=6.2\&os\_distribution=ubuntu\&os\_version=22.04\&components=server\_frontend\_agent\&db=pgsql\&ws=nginx)[https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=centos\&os\_version=8\&components=server\_frontend\_agent\&db=mysql\&ws=nginx](https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=centos\&os\_version=8\&components=server\_frontend\_agent\&db=mysql\&ws=nginx)

2\. Авторизуйтесь через браузер по указанному в процессе установки адресу, используя Admin/zabbix в качестве логина/пароля

3\. Загрузите в Zabbix шаблон из ЛК hostvm\_zabbix\_template в раздел Configuration -> Templates -> Import

4\. Добавьте хост в разделе Configuration -> Hosts -> Create Host.\
При добавлении укажите имя хоста, адрес Zabbix-агента и присоедините импортированный шаблон.

### Настройка Zabbix-агента

1\. Установите базовую конфигурацию Zabbix-агента на HOSTVM Manager с помощью инструкции с сайта разработчика [https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=centos\&os\_version=8\&components=agent\&db=\&ws=](https://www.zabbix.com/ru/download?zabbix=6.0\&os\_distribution=centos\&os\_version=8\&components=agent\&db=\&ws=)

2\. Задайте permissive SElinux режим командой:

```
semanage permissive -a zabbix_agent_t
```

3\. Добавьте правила фаервола:

```
firewall-cmd --permanent --add-port=10050/tcp
firewall-cmd --reload
```

4\. Загрузите архив на Менеджер виртуализации и распакуйте:

```
tar xvzf hostvm_zabbix_template.tar.gz

```

5\. Создайте директорию scripts:

```
mkdir -p /etc/zabbix/scripts
```

6\. Скопируйте файл `zbx-hostvm.conf` в директорию `/etc/zabbix/zabbix_agent.d/zbx-hostvm.conf`:

```
cp zbx-hostvm.conf /etc/zabbix/zabbix_agent.d/zbx-hostvm.conf
```

7\. Скопируйте файл zbx-hostvm.py в директорию /etc/zabbix/scripts/zbx-hostvm.py

```
cp zbx-hostvm.py /etc/zabbix/scripts/zbx-hostvm.py
```

8\. Установите необходимые права для файла zbx-hostvm.py:

```
chmod 0755 /etc/zabbix/scripts/zbx-hostvm.py

```

9\. Откройте файл `/etc/zabbix/zabbix_agentd.conf` и внесите изменения, пример изменяемых параметров:

```
Server = 10.1.99.30
# IP-адрес Вашего Zabbix-сервера
ServerActive = 10.1.99.30
# IP-адрес Вашего Zabbix-сервера
ListenPort = 10050
# Порт, указанный на Zabbix-сервере при добавлении хоста, в случае, если порт другой, то необходимо добавить для него соответствующее правило фаервола (см. пункт 3)
Hostname = hostvm-manager.pvhostvm.ru
# FQDN Вашего хоста, должен быть аналогичен указанному на Zabbix-сервере
Timeout=30
Include=/etc/zabbix/zabbix_agentd.d/zbx-hostvm.conf 
# В случае, если путь до конфигурационного файла другой, то его нужно изменить
```

10\. Откройте файл `/etc/zabbix/scripts/zbx-hostvm.py` и внесите изменения, указав параметры Вашей управляющей машины:

```
URL = 'https://hostvm-manager.pvhostvm.ru/ovirt-engine/api/'
USERNAME = 'admin@internal'
PASSWORD = 'HostvmManager'
CA_FILE = '/opt/certificates/ca.crt'
```

11\. Перезагрузите агент:

```
systemctl restart zabbix-agent
```

После выполненных действий дождитесь появления объектов в панели управления Zabbix.

\
