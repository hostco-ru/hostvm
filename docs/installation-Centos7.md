## Установка Centos7

### Перед установкой

Перед установкой подготовьте на вашей схд системный лун и лун для хранения виртуальных машин. 
Выполните мапинг сервера к выделенным ему лунам(vdisk'ам) по инструкциям от производителя вашего серверного оборудования.
В случае использования системного луна, подключенного по FC, настройте bootFromSan по инструкциям от производителя вашего серверного оборудования.
Подключите iso-образ к серверу, запустите сервер.

### Процесс установки
При загрузке откроется меню выбора действия. За 60 секунд выберите *Install Centos 7*. Если за 60 секунд после загрузки не вырать данный пункт, то начинается тестирование ресурсов сервера и только после этого начнется установка. Остановить тестирование ресурсов сервера возможно через нажатие клавиши *esc*.
![Картинка][image1]

В открывшемся окне выберите английский язык (English), который будет использоваться в интерфейсе установшика. 

*Выбранный язык не влияет на язык внутри самой операционной системы, которая устанавливается без графической оболочки.*

Скриншоты инструкции выполнены в интерфейсе с английским языком. Нажмите *Continue*.

![Картинка][image2]

Далее автоматически открывается меню настроек.

![Картинка][image3]

Перейдите в *DATE & TIME*, укажите ваш часовой пояс, время и дату. Нажмите *Done*.

![Картинка][image4]

Перейдите в *NETWORK & HOST NAME*. 

![Картинка][image5]

Выберите интерфейс, нажмите кнопку *Configure...*. В открывшемся окне перейдите на вкладку *IPv4 Settings*, выберите *Method: Manual*, введите ip, маску, gw, DNS-сервер. Нажмите кнопку *Save*.

![Картинка][image6]

Переведите тригер возле названия подключения в положение *On*. В поле *Host name* введите имя сервера, нажмите кнопку *Apply*. Нажмите кнопку *Done*.

![Картинка][image7]

Перейдите в *Instalation Destination*. Выберете диск на который необходимо выполнить установку. Выберете радиокнопку *I will configure partitioning*. Нажмите *Done*.

![Картинка][image8]

Если диск не отображается, необходимо открыть дополнительное окно под кнопкой *Add a disk...*, поставить галочку напротив необходимого диска и нажать кнопку *Done*.

![Картинка][image9]

После выбора места установки автоматически открывается следующее меню. 

![Картинка][image10-1]

Если диск уже использовался (имел таблицу разделов), то удалите их, как показано ниже.

![Картинка][image10-2]

![Картинка][image10-3]

Когда на диске не останется существующих разделов, нажмите *Click here to create them automatically*.

![Картинка][image10-4]

Удалите *home*, как показано ниже.

![Картинка][image10-5]

Освободившееся место отдайте разделу */*. Для этого выберете его, укажите в поле Desired Capacity его размер. Минимальный размер диска: 45GB. Измените фокус (выберете другой раздел), чтобы изменения отобразились на экране.

![Картинка][image10-6]

Нажмите *Done*.

![Картинка][image10]

Подтвердите действие кнопкой *Accept Changes*.

![Картинка][image11]

В стартовом меню нажмите кнопку *Begin Instalation*, чтобы начать установку. 

![Картинка][image12]

В открывшемся окне выберете *Root Password*. Введите ваш пароль (рекомендуемый пароль **engine**). Дважды нажмите *Done*.

![Картинка][image13]

![Картинка][image14]

Ожидайте окончания установки. После заверщения подтвердите перезагрузку нажатием на кнопку *Reboot*.

![Картинка][image15]

[image1]: ./images/centos7-install-0.jpg
[image2]: ./images/centos7-install-1.jpg
[image3]: ./images/centos7-install-2.jpg
[image4]: ./images/centos7-install-3.jpg
[image5]: ./images/centos7-install-4.jpg
[image6]: ./images/centos7-install-5.jpg
[image7]: ./images/centos7-install-6.jpg
[image8]: ./images/centos7-install-7.jpg
[image9]: ./images/centos7-install-8.jpg
[image10]: ./images/centos7-install-9.jpg
[image10-1]: ./images/centos7-install-9-1.jpg
[image10-2]: ./images/centos7-install-9-2.jpg
[image10-3]: ./images/centos7-install-9-3.jpg
[image10-4]: ./images/centos7-install-9-4.jpg
[image10-5]: ./images/centos7-install-9-5.jpg
[image10-6]: ./images/centos7-install-9-6.jpg

[image11]: ./images/centos7-install-10.jpg
[image12]: ./images/centos7-install-11.jpg
[image13]: ./images/centos7-install-12.jpg
[image14]: ./images/centos7-install-13.jpg
[image15]: ./images/centos7-install-14.jpg

### После установки

#### Проверить, что диск предназначенный для размещения виртуальных машин подключен

Командой `multipath -ll` выведете подключенные по FC устройства. Если диск нужного размера отсутствует, проверьте, что маппинг настроен верно, что на схд диск презентован серверу, что настройки диска и сервера на стороне схд выполненны верно. После этого выполните процедуру переобнаружения дисков:

1. Узнайте кол-во host bus адаптеров, которые есть на сервере:
```
# ls /sys/class/fc_host
host0  host1
```
2. Запустите сканирование:
```
echo "1" > /sys/class/fc_host/host0/issue_lip
echo "- - -" > /sys/class/scsi_host/host0/scan
echo "1" > /sys/class/fc_host/host1/issue_lip
echo "- - -" > /sys/class/scsi_host/host1/scan
```
`host0` и `host2` замените на значения, полученные в предыдушем шаге
3. Перезапустите службу multipathd
```
service multipathd restart
```
4. Проверьте, что необходимый диск стал доступен
```
multipath -ll
```

#### Проверить, что сетевые настройки выполнены в верно

Убедитесь, что внешняя сеть доступна для сервера с помощью команды `ping -c 4 <ip-адрес>`
```
[root@testname1 ~]# ping -c 4 8.8.4.4
PING 8.8.4.4 (8.8.4.4) 56(84) bytes of data.
64 bytes from 8.8.4.4: icmp_seq=1 ttl=43 time=46.8 ms
64 bytes from 8.8.4.4: icmp_seq=2 ttl=43 time=46.6 ms
64 bytes from 8.8.4.4: icmp_seq=3 ttl=43 time=46.6 ms
64 bytes from 8.8.4.4: icmp_seq=4 ttl=43 time=47.0 ms

--- 8.8.4.4 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 46.648/46.796/47.018/0.153 ms
```

Убедитесь, что имена внешней сети разрешаются:
```
[root@testname1 ~]# ping -c 4 yandex.ru
PING yandex.ru (5.255.255.70) 56(84) bytes of data.
64 bytes from yandex.ru (5.255.255.70): icmp_seq=1 ttl=52 time=31.2 ms
64 bytes from yandex.ru (5.255.255.70): icmp_seq=2 ttl=52 time=31.1 ms
64 bytes from yandex.ru (5.255.255.70): icmp_seq=3 ttl=52 time=31.1 ms
64 bytes from yandex.ru (5.255.255.70): icmp_seq=4 ttl=52 time=33.0 ms

--- yandex.ru ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 31.135/31.632/33.001/0.800 ms
```
