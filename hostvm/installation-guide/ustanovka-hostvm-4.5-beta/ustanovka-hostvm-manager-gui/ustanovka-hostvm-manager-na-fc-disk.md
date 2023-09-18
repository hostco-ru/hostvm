# Установка HOSTVM Manager на FC-диск

В случае, если установка производится без подключения к Интернету, то необходимо скачать из Личного Кабинета ISO-образ локального репозитория: hostvm-node-ng-installer-local-repo и заменить установочный диск на него.

После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root.

Пароль root для hostvm node по умолчанию: HostvmNode.

Логин HOSTVM Manager по умолчанию: admin\
Пароль HOSTVM Manager задается в процессе установки, по умолчанию HostvmManager

<figure><img src="https://lh4.googleusercontent.com/Qispe0sOmyPwyLS6LeKLo5A-ZMaqbUHr7Jx7TKHa8C0nTwxE8K8YhTzSL8oKAvIQNvHZRckwN_3-Pd-5P23o8CNXAtrthH0oud6bf9e02d7UkwkwgZ8z_hn0xOVpCo6V5PGoNbZR9K9DoolVnvIm2os" alt=""><figcaption></figcaption></figure>

Пример адреса:[ https://192.168.0.5:9090](https://192.168.0.5:9090)

Перейдите на вкладку Apps->Virtualization->Hosted HOSTVM Manager.

Напротив Hosted HOSTVM Manager нажмите кнопку Start.

{% hint style="warning" %}
Внимание! FC-диск для организации хранилища виртуальных машин должен отвечать следующим требованиям:

* объем диска – не менее 80 Gb;
* диск должен быть пустым и не содержать в себе какой-либо файловой системы;
* диск не может быть примонтирован к текущей файловой системе.
{% endhint %}

На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины HOSTVM Manager, которая будет установлена.\
Если на DNS-сервере отсутствуют записи, то их можно добавить вручную на ноде HOSTVM: [Действия при установке HOSTVM при отсутствии записей в DNS](https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns)

Заполните форму. Виртуальная машина создается со статическим файлом с использованием файла hosts.

<figure><img src="https://lh6.googleusercontent.com/DfNN2_6lW7yAm2nWdkwudevH3qqjDSkzO4K-HVqJYSE36MetD1kHJoBhS8m7Y0sGTJHKjal1tdLThx5H_hTsFh-KsjA-VhPx8zKGA_aPv9fzz11tl1tWzWPZjFXlpUEblOx0FgHyhIrHkS9P4XlAMSo" alt=""><figcaption></figcaption></figure>

Значения задаваемых параметров:

<table><thead><tr><th width="270">Параметр</th><th width="231.33333333333331">Значение</th><th>Примечание</th></tr></thead><tbody><tr><td>Engine VM FQDN</td><td>Имя будущей виртуальной машины управления виртуализацией</td><td>Имя должно разрешаться через файл <em>/etc/hosts</em> или через DNS-сервер</td></tr><tr><td>MAC Address</td><td>MAC-адрес будущей виртуальной машины управления виртуализацией</td><td>Можно оставить по умолчанию</td></tr><tr><td>Network Configuration</td><td>Способ получения IP-адреса</td><td>Static или DHCP</td></tr><tr><td>VM IP Address</td><td>Адрес виртуальной машины</td><td></td></tr><tr><td>Gateway Address</td><td>Адрес шлюза</td><td></td></tr><tr><td>DNS Servers</td><td>DNS-серверы, прописываемые внутрь виртуальной машины</td><td></td></tr><tr><td>Bridge Interface</td><td>Каким физическим интерфейсом будет пользоваться виртуальная машина</td><td>Можно оставить по умолчанию</td></tr><tr><td>Root Password</td><td>Пароль операционной системы виртуальной машины</td><td></td></tr><tr><td>Root SSH Access</td><td>Может ли пользователь root авторизоваться через SSH-сервер</td><td></td></tr><tr><td>Number of Virtual CPUs</td><td>Количество виртуальных ядер процессора, используемых виртуальной машиной</td><td>Рекомендуется не менее 4</td></tr><tr><td>Memory Size (MiB)</td><td>Память, используемая виртуальной машиной</td><td>Не менее 4096</td></tr><tr><td>Root SSH Public Key</td><td>SSH-ключ безпарольного доступа внутрь виртуальной машины</td><td></td></tr><tr><td>Bridge Name</td><td>Имя виртуального сетевого интерфейса виртуальной машины</td><td>Рекомендуется оставить по умолчанию</td></tr><tr><td>Gateway Address</td><td>Шлюз виртуальной машины</td><td>Рекомендуется оставить по умолчанию</td></tr><tr><td>Host FQDN</td><td>Имя хоста виртуализации</td><td>Рекомендуется оставить по умолчанию</td></tr><tr><td>Edit Hosts File</td><td>Создавать ли записи в hosts виртуальной машины</td><td>Рекомендуется включать опцию</td></tr><tr><td>Pause Host</td><td>Требуется ли приостанавливать установку для внесения своих изменений в настройки виртуальной машины</td><td>Не рекомендуется включать опцию</td></tr><tr><td>Apply OpenSCAP profile</td><td>Профиль OpenSCP</td><td>Не рекомендуется включать опцию</td></tr><tr><td>Network Test</td><td>Проверка работоспособности виртуальной машины в процессе установки</td><td>Рекомендуется выбрать <em>ping</em> при использовании файла hosts. Можно оставить <em>DNS</em> при использовании DNS</td></tr><tr><td>OVA Archive Path</td><td>Путь до альтернативного образа виртуальной машины</td><td>Не рекомендуется использовать</td></tr></tbody></table>

Заполните следующую форму. Укажите пароль от веб-интерфейса будущей виртуальной машины. Заполните настройки отправки уведомлений e-mail при необходимости.

<figure><img src="https://lh6.googleusercontent.com/9SQku4mRqjDvU7chUuRbZ6p3ycvfH-8hs9Mj-dYmPYbQZWzkTLJL1a6ggvSKZc5LOsBnoyhh70CrJf4wtkD9feX0XwUc7J87sWIgxA0I5GNLINommucu6pJKsqZHjgyZLaVc5KTXZdR_MTCHyPQo594" alt=""><figcaption></figcaption></figure>

Создайте управляющую виртуальную машину (нажмите Prepare VM).

<figure><img src="https://lh5.googleusercontent.com/09_EJ5UQbJ3HILO7qWuMnTON3HDd7X3t4QJWXO52jt5pQy3u48WqlwLUHY7j3mmcUkH0zs6z0dAJ1MFGieo6PERPMFlBT6IwsE9TYIXo9D-5TaDqxZyt0gFg4o3w01eFEvc6fQZ7TYTzOpcJto4KukM" alt=""><figcaption></figcaption></figure>

Подождите довольно продолжительное время, пока виртуальная машина не будет создана и настроена.

<figure><img src="https://lh4.googleusercontent.com/CjF8KlUHfW9VzYJtCTJb5qrvL0RKi5sCV2eCEn09bawdX972VrmlFAPAUtc3BDyAs26mpwJvce9EpGGODR3TYNpidDC5cN0q0SuY__6hgi9KzA9jUn8UcOH9DfhXNV1gvKORwC_Z7eAv16II0Gz773s" alt=""><figcaption></figcaption></figure>

Выберете тип хранилища Fibre Channel и укажите свободный LUN.

<figure><img src="https://lh4.googleusercontent.com/m2qJo8xkRb9FGnXlyp97q0QmtirAlwY30Ubpe7NIVGK6EjVtrS0ezbQoOMl8w0fsEH54m5sCiXf2419qu8g4LjbTlX08l0fGtwAAoOlOBdvQzpH081WLpbVwucD_PySWLFigRk3FIfciJvSrLFC54-M" alt=""><figcaption></figcaption></figure>

Подождите, пока hosted HOSTVM Manager расположится в хранилище.

<figure><img src="https://lh4.googleusercontent.com/wR_CK5-pzu31_f_XOqP0XzC3t7TbLirKmRpvSiegFljYdg0MQW5Fd6lUcWQx_YAISHbo4Cfugvt5pXtPboEXyc1YUi_UJIOO6AvdCylyIXguLqbYG911Fnj0qUe360vqsJjOUG40hRNYD9c3x9PBhpg" alt=""><figcaption></figcaption></figure>

Поздравляем! Hosted HOSTVM Manager развернут.

<figure><img src="https://lh6.googleusercontent.com/yWQoFDIq5zGXZRoJUCK5QsG2tmlOeLCb0cIZoFE3Ni59s-giSBY0_6hCbSA1xAexHDkA-rE6XYTPOaLOjqKuPxxuaZ3Mn_yXSi1rR_0mFzTekwaKtSwASoc2c6hjUVJNH2FnoK4tCgSZukURpmhmvas" alt=""><figcaption></figcaption></figure>
