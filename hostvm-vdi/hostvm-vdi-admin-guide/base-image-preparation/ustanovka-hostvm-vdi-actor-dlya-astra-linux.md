# Установка HOSTVM VDI Actor для Astra Linux



**(только для Astra Linux 1.7): Добавить репозитории Astra Linux**

```bash
echo 'deb http://download.astralinux.ru/astra/stable/1.7_x86-64/repository-base/ 1.7_x86-64 main contrib non-free' | sudo tee -a /etc/apt/sources.list
```

<details>

<summary>Альтернативный репозиторий</summary>

```bash
echo 'deb http://dl.astralinux.ru/astra/stable/1.7_x86-64/repository-base/ 1.7_x86-64 main contrib non-free' | sudo tee -a /etc/apt/sources.li
```

</details>

**Настройка виртуального окружения:**

```bash
sudo apt update
```

```bash
sudo apt install -y python3-pip python3-setuptools python3.7-venv python3-six python3-requests 
```

```bash
cd /var
```

```bash
sudo python3.7 -m venv astra-venv
```

```bash
source /var/astra-venv/bin/activate
```

```bash
sudo chmod -R a+rwx /var/astra-venv
```

```bash
python -m pip install --upgrade pip
```

```bash
pip install PyQt5 pyqt5-tools requests
```

```bash
sudo ln -s /usr/lib/x86_64-linux-gnu/libxcb-util.so.0 /usr/lib/x86_64-linux-gnu/libxcb-util.so.1
```

**Установка зависимостей:**

<details>

<summary>Рекомендуемый способ без использования Debian репозиториев</summary>

Загрузить пакеты xscreensaver, xscreensaver-data из [личного кабинета](https://lk.pvhostvm.ru/Download) на виртуальную машину.\
Найти нужные пакеты можно по следующему пути:

`Каталог загрузок/Дистрибутивы/HOSTVM VDI/Actor dependencies/Astra Linux 1.6`

или

`Каталог загрузок/Дистрибутивы/HOSTVM VDI/Actor dependencies/Astra Linux 1.7`, соответственно.

Выдать пользователю \_apt права на загруженные пакеты:

```bash
sudo chown _apt xscreensaver_5.*amd64.deb ; sudo chown _apt xscreensaver-data_5*amd64.deb 
```

Произвести установку, с удовлетворением зависимостей из репозиториев Astra Linux:

```bash
sudo apt install -y ./xscreensaver-data_5*amd64.deb ; sudo apt install -y ./xscreensaver_5*amd64.deb
```

</details>

<details>

<summary>Альтернативный способ с использованием репозиториев Debian</summary>

**Для установки пакета xscreensaver нужно добавить репозитории debian соответствующие версии Astra Linux:**

**(только для Astra Linux 1.6): Добавить репозитории Debian 9**

```bash
echo 'deb http://deb.debian.org/debian/ stretch main' | sudo tee -a /etc/apt/sources.list
echo 'deb-src  http://deb.debian.org/debian/ stretch main' | sudo tee -a /etc/apt/sources.list	
```

**(только для Astra Linux 1.7): Добавить репозитории Debian 10**

```bash
echo 'deb http://ftp.debian.org/debian buster main contrib non-free' |sudo tee -a /etc/apt/sources.list
echo 'deb-src http://ftp.debian.org/debian buster main contrib non-free' |sudo tee -a /etc/apt/sources.list
```

```bash
sudo apt update 
```

Добавить нужные ключи (если появятся соотв. уведомления):

```bash
sudo apt-key adv --recv-key --keyserver keyserver.ubuntu.com {PUB_KEY}
```

```bash
sudo apt update 
```

```bash
sudo apt install -y xscreensaver			
```

</details>

**Загрузить пакет udsactor на хост:**

Загрузить актуальный пакет актора с расширением .deb на хост. Пакет доступен для загрузки в веб-интерфейсе брокера HOSTVM VDI из-под учетной записи с правами администратора. Более подробно процесс получения пакета отражен в разделе [VDI Actor](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-admin-guide/hostvm-vdi-base-image-preparation).

**Установить пакет udsactor на хост:**

```bash
sudo dpkg -i /home/astra/udsactor_3.0.0_all.deb
```

Ожидаемый вывод: подпроцесс установлен сценарий post-installation возвратил код ошибки 1

**Редактирование конфигов актора для виртуального окружения:**

```bash
sudo sed -i  '/FOLDER=\/usr\/share\/UDSActor/i source \/var\/astra-venv\/bin\/activate' /usr/bin/udsactor
```

```bash
sudo sed -i  '/FOLDER=\/usr\/share\/UDSActor/i source \/var\/astra-venv\/bin\/activate' /usr/sbin/UDSActorConfig
```

```bash
sudo sed -i 's/exec python3/exec python/i' /usr/bin/udsactor	
```

```bash
sudo sed -i 's/exec python3/exec python/i' /usr/sbin/UDSActorConfig	
```

**Проверить работоспособность конфигов:**

```bash
sudo /usr/bin/udsactor 
```

ожидаемый вывод: usage: udsactor start|stop|restart|login "username"|logout "username"

```bash
sudo /usr/sbin/UDSActorConfig	
```

ожидаемый вывод, если подключены через ssh:

```bash
 qt.qpa.xcb: could not connect to display
```

ожидаемый вывод, если подключены с графикой:

```
открытие UDS Actor Configuration Tool
```
