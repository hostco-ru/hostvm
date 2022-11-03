# Установка HOSTVM  VDI Actor для Astra Linux

Данное руководство описывает шаги, необходимые для установки HOSTVM VDI Actor в золотой образ на базе Astra Linux.\
\


**(только для Astra Linux 1.7): Добавить репозитории Astra Linux**

```
echo 'deb http://download.astralinux.ru/astra/stable/1.7_x86-64/repository-base/ 1.7_x86-64 main contrib non-free' | sudo tee -a /etc/apt/sources.list
```

**Настройка виртуального окружения:**

```
sudo apt update
```

```
sudo apt install -y python3-pip python3-setuptools python3.7-venv python3-six python3-requests 
```

```
cd /var
```

```
sudo python3.7 -m venv astra-venv
```

```
source /var/astra-venv/bin/activate
```

```
sudo chmod -R a+rwx /var/astra-venv
```

```
python -m pip install --upgrade pip
```

```
pip install PyQt5 pyqt5-tools requests
```

```
sudo ln -s /usr/lib/x86_64-linux-gnu/libxcb-util.so.0 /usr/lib/x86_64-linux-gnu/libxcb-util.so.1
```

**Для установки пакета xscreensaver нужно добавить репозитории debian соответствующие версии Astra Linux:**

**(только для Astra Linux 1.6): Добавить репозитории Debian 9**

```
echo 'deb http://deb.debian.org/debian/ stretch main' | sudo tee -a /etc/apt/sources.list
echo 'deb-src  http://deb.debian.org/debian/ stretch main' | sudo tee -a /etc/apt/sources.list	
```

**(только для Astra Linux 1.7): Добавить репозитории Debian 10**

```
echo 'deb http://ftp.debian.org/debian buster main contrib non-free' |sudo tee -a /etc/apt/sources.list
echo 'deb-src http://ftp.debian.org/debian buster main contrib non-free' |sudo tee -a /etc/apt/sources.list
```

```
sudo apt update 
```

Добавить нужные ключи (если появятся соотв. уведомления):

```
sudo apt-key adv --recv-key --keyserver keyserver.ubuntu.com {PUB_KEY}
```

```
sudo apt update 
```

```
sudo apt install -y xscreensaver			
```

**Загрузить пакет udsactor на хост:**

[Согласно документации](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-admin-guide/hostvm-vdi-base-image-preparation)

**Установить пакет udsactor на хост:**

```
sudo dpkg -i /home/astra/udsactor_3.0.0_all.deb
```

Ожидаемый вывод: подпроцесс установлен сценарий post-installation возвратил код ошибки 1

**Редактирование конфигов актора для виртуального окружения:**

```
sudo sed -i  '/FOLDER=\/usr\/share\/UDSActor/i source \/var\/astra-venv\/bin\/activate' /usr/bin/udsactor
```

```
sudo sed -i  '/FOLDER=\/usr\/share\/UDSActor/i source \/var\/astra-venv\/bin\/activate' /usr/sbin/UDSActorConfig
```

```
sudo sed -i 's/exec python3/exec python/i' /usr/bin/udsactor	
```

```
sudo sed -i 's/exec python3/exec python/i' /usr/sbin/UDSActorConfig	
```

**Проверить работоспособность конфигов:**

```
sudo /usr/bin/udsactor 
```

ожидаемый вывод: usage: udsactor start|stop|restart|login "username"|logout "username"

```
sudo /usr/sbin/UDSActorConfig	
```

ожидаемый вывод, если подключены через ssh:

```
 qt.qpa.xcb: could not connect to display
```

ожидаемый вывод, если подключены с графикой:

```
открытие UDS Actor Configuration Tool
```
