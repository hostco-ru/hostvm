# Установка HOSTVM Manager на NFS

В случае, если установка производится без подключения к Интернету, то необходимо скачать из Личного Кабинета ISO-образ локального репозитория: hostvm-node-ng-installer-local-repo и заменить установочный диск на него.

После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root.

Пароль root для hostvm node по умолчанию: HostvmNode.

Логин HOSTVM Manager по умолчанию: admin\
Пароль HOSTVM Manager задается в процессе установки, по умолчанию HostvmManager

На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины HOSTVM Manager, которая будет установлена.\
\
Если на DNS-сервере отсутствуют записи, то их можно добавить вручную на ноде HOSTVM: [Действия при установке HOSTVM при отсуствии записей в DNS](https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns)

Перейдите на вкладку Apps->Virtualization->Hosted HOSTVM Manager.

Напротив Hosted HOSTVM Manager нажмите кнопку Start.

<figure><img src="https://lh6.googleusercontent.com/DOCbGogon7SzJfrv2Ke44zx9NT4IIcoX_2-I4ucVq_Tcfg1gUj16oBrsOUqlAD0hay20JK57MyLeqZxP42jPcvYMdVO5PUMtV_yMCbLnvrkQa8qG7Md471-LrlyRFCGrmmTKo5_4aeb2djSSEgYw9Bc" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Внимание! Объём NFS-хранилища должен быть не менее 80 Гб.
{% endhint %}

Заполните форму. Виртуальная машина создается со статическим файлом с использованием файла hosts.

<figure><img src="https://lh6.googleusercontent.com/DfNN2_6lW7yAm2nWdkwudevH3qqjDSkzO4K-HVqJYSE36MetD1kHJoBhS8m7Y0sGTJHKjal1tdLThx5H_hTsFh-KsjA-VhPx8zKGA_aPv9fzz11tl1tWzWPZjFXlpUEblOx0FgHyhIrHkS9P4XlAMSo" alt=""><figcaption></figcaption></figure>

\
Значения задаваемых параметров:

| Параметр               | Значение                                                                                            | Примечание                                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Engine VM FQDN         | Имя будущей виртуальной машины управления виртуализацией                                            | Имя должно разрешаться через файл _/etc/hosts_ или через DNS-сервер                                    |
| MAC Address            | MAC-адрес будущей виртуальной машины управления виртуализацией                                      | Можно оставить по умолчанию                                                                            |
| Network Configuration  | Способ получения IP-адреса                                                                          | Static или DHCP                                                                                        |
| VM IP Address          | Адрес виртуальной машины                                                                            |                                                                                                        |
| Gateway Address        | Адрес шлюза                                                                                         |                                                                                                        |
| DNS Servers            | DNS-серверы, прописываемые внутрь виртуальной машины                                                |                                                                                                        |
| Bridge Interface       | Каким физическим интерфейсом будет пользоваться виртуальная машина                                  | Можно оставить по умолчанию                                                                            |
| Root Password          | Пароль операционной системы виртуальной машины                                                      |                                                                                                        |
| Root SSH Access        | Может ли пользователь root авторизоваться через SSH-сервер                                          |                                                                                                        |
| Number of Virtual CPUs | Количество виртуальных ядер процессора, используемых виртуальной машиной                            | Рекомендуется не менее 4                                                                               |
| Memory Size (MiB)      | Память, используемая виртуальной машиной                                                            | Не менее 4096                                                                                          |
| Root SSH Public Key    | SSH-ключ безпарольного доступа внутрь виртуальной машины                                            |                                                                                                        |
| Bridge Name            | Имя виртуального сетевого интерфейса виртуальной машины                                             | Рекомендуется оставить по умолчанию                                                                    |
| Gateway Address        | Шлюз виртуальной машины                                                                             | Рекомендуется оставить по умолчанию                                                                    |
| Host FQDN              | Имя хоста виртуализации                                                                             | Рекомендуется оставить по умолчанию                                                                    |
| Edit Hosts File        | Создавать ли записи в hosts виртуальной машины                                                      | Рекомендуется включать опцию                                                                           |
| Pause Host             | Требуется ли приостанавливать установку для внесения своих изменений в настройки виртуальной машины | Не рекомендуется включать опцию                                                                        |
| Apply OpenSCAP profile | Профиль OpenSCP                                                                                     | Не рекомендуется включать опцию                                                                        |
| Network Test           | Проверка работоспособности виртуальной машины в процессе установки                                  | Рекомендуется выбрать _ping_ при использовании файла hosts. Можно оставить _DNS_ при использовании DNS |
| OVA Archive Path       | Путь до альтернативного образа виртуальной машины                                                   | Не рекомендуется использовать                                                                          |

Заполните следующую форму. Укажите пароль от веб-интерфейса будущей виртуальной машины. Заполните настройки отправки уведомлений e-mail при необходимости.

<figure><img src="https://lh6.googleusercontent.com/9SQku4mRqjDvU7chUuRbZ6p3ycvfH-8hs9Mj-dYmPYbQZWzkTLJL1a6ggvSKZc5LOsBnoyhh70CrJf4wtkD9feX0XwUc7J87sWIgxA0I5GNLINommucu6pJKsqZHjgyZLaVc5KTXZdR_MTCHyPQo594" alt=""><figcaption></figcaption></figure>

Создайте управляющую виртуальную машину (нажмите Prepare VM).

<figure><img src="https://lh5.googleusercontent.com/09_EJ5UQbJ3HILO7qWuMnTON3HDd7X3t4QJWXO52jt5pQy3u48WqlwLUHY7j3mmcUkH0zs6z0dAJ1MFGieo6PERPMFlBT6IwsE9TYIXo9D-5TaDqxZyt0gFg4o3w01eFEvc6fQZ7TYTzOpcJto4KukM" alt=""><figcaption></figcaption></figure>

Подождите довольно продолжительное время, пока виртуальная машина не будет создана и настроена.

<figure><img src="https://lh4.googleusercontent.com/gYLTMeGNDEbeDwP09hdQOag9F5oZg8o2iFoRJSRmLCrwzMapnfvwcDfZRWsC9Ibrln8-Midk6yuK21IE6jGYg8K6AwH0PrR9EGUACbkGejwbLyz0LSJG6T1NcOxMqNo_j1SzMdMzeU81K_22dLNcJao" alt=""><figcaption></figcaption></figure>

Выберете тип хранилища NFS и укажите путь экспорта.

<figure><img src="https://lh6.googleusercontent.com/JDvHzccXOsm-BSwlOGqV7P5N5BDgsNo1qDv8Vqjb2OKlT8zWuuOAlVslRYa6ZfXC4-sMrOiYu63rWrxV4Dz7_lSVpEqxIAPuYHcGy8hj-_E1L8qu7BIbKePZ3XqgqIyDNYDari3FJvePShb2eueNGp8" alt=""><figcaption></figcaption></figure>

Подождите пока Hosted HOSTVM Manager расположится в хранилище.

<figure><img src="https://lh6.googleusercontent.com/MbkeXLgi4uEuO_f7RB_ePWoAWOsbNAY4Uiz_AfVEhapbGM7YXCTm6bzX_YshntpJCEnp0-Bn81nvazB-qVHnIKzCK5IKvqEFiqwORKWtouIUvZQry2kJQgiQaa2H7Q-KhWpFrU89t6jNhwx8JDABv3I" alt=""><figcaption></figcaption></figure>

Поздравляем! Hosted HOSTVM Manager развернут.

<figure><img src="https://lh6.googleusercontent.com/yWQoFDIq5zGXZRoJUCK5QsG2tmlOeLCb0cIZoFE3Ni59s-giSBY0_6hCbSA1xAexHDkA-rE6XYTPOaLOjqKuPxxuaZ3Mn_yXSi1rR_0mFzTekwaKtSwASoc2c6hjUVJNH2FnoK4tCgSZukURpmhmvas" alt=""><figcaption></figcaption></figure>

<br>

<br>
