# Ввод тонких клонов в домен для Astra Linux

**Все действия выполняются в ОС золотого образа.**

Создать файл скрипта postinstall.sh с содержанием:

```bash
sudo astra-ad-sssd-client -d 'нужный_домен' -u 'логин_доменной_уч'  -p 'пароль_доменной_уч' -y ; sudo reboot -h now
```

Добавить разрешение на запуск созданному файлу, например:

```bash
sudo chmod +x /home/astra/postinstall.sh
```

В Actor Configuration Tool на вкладке Advanced указать:

```bash
sh /путь/postinstall.sh
```

Например:\


<figure><img src="../../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

Применить новую конфигурацию нажав **Register with UDS**.

Выполнить публикацию на брокере.
