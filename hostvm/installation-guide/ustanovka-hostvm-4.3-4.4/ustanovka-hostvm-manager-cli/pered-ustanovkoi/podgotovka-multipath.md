# Подготовка multipath

На хосте выполните следующие команды:

```
[root@testname1 ~]# mpathconf --enable
[root@testname1 ~]# systemctl enable multipathd
[root@testname1 ~]# systemctl start multipathd
```

Затем перезапустите хост и проверьте, что multipath активен и LUN'ы отображаются

```
[root@testname1 ~]# systemctl status multipathd
[root@testname1 ~]# multipath -ll

```

## Проверить, что диск предназначенный для размещения виртуальных машин подключен

Командой `multipath -ll` выведете подключенные по FC устройства. Если диск нужного размера отсутствует, проверьте, что маппинг настроен верно, что на схд диск презентован серверу, что настройки диска и сервера на стороне схд выполнены верно. После этого выполните процедуру переобнаружения дисков:

1.  Узнайте количество host bus адаптеров, которые есть на сервере:

    ```
    # ls /sys/class/fc_host
    host0  host1
    ```
2.  Запустите сканирование:

    ```
    echo "1" > /sys/class/fc_host/host0/issue_lip
    echo "- - -" > /sys/class/scsi_host/host0/scan
    echo "1" > /sys/class/fc_host/host1/issue_lip
    echo "- - -" > /sys/class/scsi_host/host1/scan
    ```

    `host0` и `host2` замените на значения, полученные в предыдущем шаге
3.  Перезапустите службу multipathd

    ```
    service multipathd restart
    ```
4.  Проверьте, что необходимый диск стал доступен

    ```
    multipath -ll
    ```
