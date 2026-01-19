# Оффлайн установка

Данное руководство описывает поэтапный процесс установки и настройки агента HOSTVM VDI в операционной системе Astra Linux в условиях отсутствия доступа к сети Интернет.

### Подготовительный этап <a href="#prep" id="prep"></a>

Для загрузки всех нужных зависимостей и компонентов потребуется машина с установленным дистрибутивом Astra Linux и доступом к сети Интернет.

Для Astra Linux 1.7 уже есть готовый архив для установки, его можно загрузить из [личного кабинета](https://lk.pvhostvm.ru/Download) . Найти нужные пакеты можно по следующему пути: `Каталог загрузок/Дистрибутивы/HOSTVM VDI/Misc/Actor dependencies/Astra Linux 1.7`.

**Настройка кеширующего прокси apt-cacher-ng:**

{% hint style="info" %}
Для корректной работы apt-cacher-ng необходимо использовать http-репозитории
{% endhint %}

```bash
sudo apt update
```

```bash
sudo apt install -y apt-cacher-ng
```

```bash
sudo systemctl enable --now apt-cacher-ng
```

```bash
echo 'Acquire::http { Proxy "http://127.0.0.1:3142"; };' | sudo tee /etc/apt/apt.conf.d/01proxy
```

```bash
sudo sed -i 's/^#PassThroughPattern:/PassThroughPattern:/' /etc/apt-cacher-ng/acng.conf
```

```bash
sudo sed -i 's/^PassThroughPattern:.*/PassThroughPattern: .*/' /etc/apt-cacher-ng/acng.conf
```

```bash
sudo systemctl restart apt-cacher-ng
```

Установка Python и зависимостей Actor: выполните установку по [стандартной инструкции](https://kb.pvhostvm.ru/hostvm-vdi/hostvm-vdi-installation-guide/actor/ustanovka-hostvm-vdi-actor-dlya-astra-linux).

**Формирование оффлайн‑репозитория пакетов Astra:**

```bash
mkdir -p ./astra-offline-repo/
sudo find /var/cache/apt-cacher-ng/download.astralinux.ru/astra/stable/1.7_x86-64/ -name "*.deb" -exec cp {} ./astra-offline-repo/ \;
```

**Загрузка оффлайн-пакетов pip:**

```bash
source /var/astra-venv/bin/activate
```

```bash
mkdir -p ./offline-pip-packages
```

```bash
pip download -d ./offline-pip-packages PyQt5 pyqt5-tools requests pip setuptools wheel
```

**Создание установочного скрипта installer.sh:**

```bash
cat > installer.sh << 'EOF'
#!/bin/bash
sudo dpkg -i ./astra-offline-repo/*.deb 2>/dev/null
sudo python3.7 -m venv /var/astra-venv
source /var/astra-venv/bin/activate
sudo chmod -R a+rwx /var/astra-venv
python -m pip install --no-index --find-links=./offline-pip-packages pip --upgrade
pip install --no-index --find-links=./offline-pip-packages PyQt5 pyqt5-tools requests
sudo dpkg --clear-avail
sudo dpkg -i ./astra-offline-repo/*.deb 2>/dev/null || sudo apt-get install -f -y
sudo chown _apt xscreensaver_*amd64.deb
sudo chown _apt xscreensaver-data*amd64.deb
sudo apt install -y ./xscreensaver-data*amd64.deb
sudo apt install -y ./xscreensaver_*amd64.deb
sudo dpkg -i ./udsactor*all.deb
EOF
```

```bash
chmod +x installer.sh
```

**Создание архива оффлайн‑репозитория:**

```bash
sudo tar -czvf offline-repo.tar.gz \
  ./astra-offline-repo/ ./offline-pip-packages \
  ./xscreensaver_*amd64.deb ./xscreensaver-data_*amd64.deb \
  ./udsactor_*all.deb \
  installer.sh
```

### Установка на Astra Linux <a href="#install" id="install"></a>

После завершения подготовительного этапа на целевой машине распакуйте архив и выполните скрипт установки:

```
tar -xzvf offline-repo.tar.gz
chmod +x installer.sh
./installer.sh
```

Установка завершена!
