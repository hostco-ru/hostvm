# Установка на Astra Linux

### Установка HOSTVM VDI Actor на Astra Linux 1.6 и Astra Linux 1.7&#x20;

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

`Каталог загрузок/Дистрибутивы/HOSTVM VDI/Misc/Actor dependencies/Astra Linux 1.6`

или

`Каталог загрузок/Дистрибутивы/HOSTVM VDI/Misc/Actor dependencies/Astra Linux 1.7`, соответственно.

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

Загрузить актуальный пакет актора с расширением .deb на хост. Пакет доступен для загрузки в веб-интерфейсе брокера HOSTVM VDI из-под учетной записи с правами администратора. Более подробно процесс получения пакета отражен в разделе [Агент HOSTVM VDI](./).

**Установить пакет udsactor на хост:**

```bash
sudo dpkg -i /home/astra/udsactor_3.0.0_all.deb
```

Установка завершена! В случае возникновения ошибки с кодом 1, необходимо отредактировать конфигурацию и проверить работоспособность. Подробно процесс описан ниже в разделе "Редактирование конфигурации при наличии ошибок в процессе установки".

### Установка HOSTVM VDI Actor на Astra Linux 1.8

**Настройка виртуального окружения:**

```
sudo apt update
```

```
sudo apt install -y python3-pip python3-setuptools python3.11-venv python3-six python3-requests 
```

```
cd /var
```

```
sudo python3.11 -m venv astra-venv
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

**Установка зависимостей:**

Загрузить пакеты xscreensaver, xscreensaver-data из [личного кабинета](https://lk.pvhostvm.ru/Download) на виртуальную машину.\
Найти нужные пакеты можно по следующему пути:

Каталог загрузок/Дистрибутивы/HOSTVM VDI/Misc/Actor dependencies/Astra Linux 1.8

Выдать пользователю \_apt права на загруженные пакеты:

```
sudo chown _apt xscreensaver_6.06+dfsg1-3+deb12u1_amd64.deb 
```

```
sudo chown _apt xscreensaver-data_6.06+dfsg1-3+deb12u1_amd64.deb 
```

Произвести установку, с удовлетворением зависимостей из репозиториев Astra Linux:

```
sudo apt install -y ./xscreensaver-data_6.06+dfsg1-3+deb12u1_amd64.deb
```

```
sudo apt install -y ./xscreensaver_6.06+dfsg1-3+deb12u1_amd64.deb
```

**Загрузить пакет udsactor на хост:**

Загрузить актуальный пакет актора с расширением .deb на хост. Пакет доступен для загрузки в веб-интерфейсе брокера HOSTVM VDI из-под учетной записи с правами администратора. Более подробно процесс получения пакета отражен в разделе [Агент HOSTVM VDI](./).

**Установить пакет udsactor на хост:**

```
sudo dpkg -i udsactor_3.6.0_all.deb
```

Установка завершена! В случае возникновения ошибки с кодом 1, необходимо отредактировать конфигурацию и проверить работоспособность. Подробно процесс описан ниже в разделе "Редактирование конфигурации при наличии ошибок в процессе установки".

### Редактирование конфигурации при наличии ошибок в процессе установки

**Отредактируйте конфигурацию актора для виртуального окружения, выполнив следующие команды:**

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

**Проверьте работоспособность конфигурации:**

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
