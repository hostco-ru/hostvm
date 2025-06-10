# Установка HOSTVM Manager на Gluster

В случае, если установка производится без подключения к Интернету, то необходимо скачать из Личного Кабинета ISO-образ локального репозитория: hostvm-node-ng-installer-local-repo и заменить установочный диск на него.

После установки ISO-дистрибутива на сервер обратитесь по адресу, указанному в процессе установки на порт 9090 через браузер и зайдите под пользователем root.

Пароль root для hostvm node по умолчанию: HostvmNode.

Логин HOSTVM Manager по умолчанию: admin\
Пароль HOSTVM Manager задается в процессе установки, по умолчанию HostvmManager

Пример адреса: https://192.168.0.5:9090\
\
На DNS-сервере должны быть как минимум две записи типа A, содержащие в себе FQDN-имя сервера, а также имя виртуальной машины HOSTVM Manager, которая будет установлена.\
Если на DNS-сервере отсутствуют записи, то их можно добавить вручную на ноде HOSTVM: [Действия при установке HOSTVM при отсутствии записей в DNS](https://kb.pvhostvm.ru/hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns)

Перейдите на вкладку Терминал и поочередно введите команды:

```
root@testname1 ~]# ssh-keygen
root@testname1 ~]# ssh-copy-id root@test-454
```

\*root - имя пользователя\
\
\*test-454 - FQDN хоста, на котором будет хранилище

<figure><img src="https://lh3.googleusercontent.com/k1qfq1PsYuTGKjqVYBRKRW9_aAwPRnxIVx7HJaLWlzp7vWJ5fDdeJrug8-GCar5r_mFOnqC7rnw0w9ia8RHPJiQsMHDBqLcJEUTN_U3fss01oCT3agJhFdgaEIwyAjFJAH_Da9ps3Il0K2wjYrguQ3A" alt=""><figcaption></figcaption></figure>

Далее нужно создать раздел, для этого заходим в Хранилище -> Создать раздел

<figure><img src="https://lh3.googleusercontent.com/dFK6IlXfLNztGZx7tdXrLSHcJM_44dGp1lLv2yOQ_aF54_vZ-Y6aTo8zuXUEWcoSAIl5Z8YHOyuiPhFBm5ykT62_n8K1tYmC69Ju--wA0OsumM33mxAVbQLFduNm0tOrTnEI_qdErfUgtxOtHa2TsD8" alt=""><figcaption></figcaption></figure>

Задаем параметры, как на скриншоте ниже, определяем размер раздела (не меньше 90 GB). Тип раздела выбираем Нет файловой системы.

<figure><img src="https://lh5.googleusercontent.com/Jr7sSQEKpc7B25p1imdruYu0XyGClKZmVY8Ikhyvc2ZXiVDVrlwFoJlnZHvGprD-AHnD7TL3WqkTd9KiEJ9EF9QaKqPk7CYGjOX7HxTgBnLMNkYZWWkoydgfD362JwuPDwwoa70rjrsUbAzyCLbK760" alt=""><figcaption></figcaption></figure>

Запоминаем, какой раздел был создан, в нашем случае /dev/sda3

<figure><img src="https://lh4.googleusercontent.com/1YuPdDOEoa-QZpKE2nRJQkmSO78WrX6g3wtWcZJwWkCId1mAehrKvAakOszOE0sMc-lBn4MTakcAlh_aipse3F7MmWytC2xtTbnRMi0qmDMvgi3W37FfD0jYTbfOi8ath2xVCjnIpsnKUm0LWv2L4JQ" alt=""><figcaption></figcaption></figure>

Далее заходим в Virtualization -> Hosted HOSTVM Manager -> Hyperconverged

<figure><img src="https://lh6.googleusercontent.com/DOCbGogon7SzJfrv2Ke44zx9NT4IIcoX_2-I4ucVq_Tcfg1gUj16oBrsOUqlAD0hay20JK57MyLeqZxP42jPcvYMdVO5PUMtV_yMCbLnvrkQa8qG7Md471-LrlyRFCGrmmTKo5_4aeb2djSSEgYw9Bc" alt=""><figcaption></figcaption></figure>

Выбираем Run Gluster Wizard For Single Node для установки хранилища на одной ноде

<figure><img src="https://lh3.googleusercontent.com/siCv1rZLaEeadxC5Yr7Xe9kCSGYaegpXrUwtTa-gqwNJy8UOrfyx-Zj_jYgIY0JWk0ypUVmo4yOnaa4G5d6VtF16UUSa5hOuvzKtd7n6Vum93huzVSNtp0yLbUrPcNrlgOkQ972BwhkyyZJCY0RiO5s" alt=""><figcaption></figcaption></figure>

Вводим имя хоста, для которого генерировали ключи, жмем next

<figure><img src="https://lh5.googleusercontent.com/8QIiKblHWqs3QdDAfy8hD44Xz0ncxsA495MMhU-3AL-z1hzAQiMoSJmjCtcMPVHKe0-vlFq3SVjN3VeUEp-QiYK5g05m086vPhAh5-kYPSSKP0gvNhDhhhSFOgID3Nyih_2kGoS38JmAQQH7OzQqVFU" alt=""><figcaption></figcaption></figure>

Оставляем как есть, жмем next

<figure><img src="https://lh3.googleusercontent.com/LGi3g0NTZ6RHht9IRUyRbC49rrHa_Qr0ZFAKLeV7oUJJ5zYkw8el_9BP_qFBl26CCbyT8r8exWtT8UeTjUJ92ikRapKUr3VUSYQ-1kiOob-QY5-FGiSScnb5Nbr0aHMffmQ4SVhqVATImF1UYWbD0LU" alt=""><figcaption></figcaption></figure>

Удаляем data и vmstore, жмем next

<figure><img src="https://lh5.googleusercontent.com/W8OsmHe518S57MZoKtZAAHkxTJRWLP3mUQ6ySFTY4zN70FinqYhzXlAAEZVqAXwJ827KzHDs3yGYfzwc0ZuoP3bWXde-sAIpITi-9M5FA_O1Sk7i2gWNpz0MZcj_OvqUU6AFon2JChb2Bbat_HRKo60" alt=""><figcaption></figcaption></figure>

·         Выбираем нужный тип RAID

·         В Device Name выбираем раздел, который был создан ранее (/dev/sda3)

·         Задаем размер LV Size в соответствии с системными требованиями, жмем next.\


<figure><img src="https://lh4.googleusercontent.com/xlrNB68xo718FzRkwEzFwtieWuq5uNBHZPwM0mKIrCnfkm7FdM3EyTQY0u1-kcfWEeH5DLIwmmlfI1gckv1jQR5j2stzu7wRQPfhRVzVX3cWR4N2tohCGiFEzzHW-i5C7WiHs5p6aSm4Jcc6Hr-b718" alt=""><figcaption></figcaption></figure>

Нажимаем deploy

<figure><img src="https://lh6.googleusercontent.com/9IiChUBLnbfZrjWkajrDRiELMuyxv4BecPqgOicLE-b4GuDBxCMX4xsuSR4n451iPUT8qDgUycaYJbEXVd48GG8J-ptmaiUHTvBPfBgXwoJb_V8TJZhdrA-IECSoOASIVbZTvWOxgiKQ5uSKpwgOprg" alt=""><figcaption></figcaption></figure>

Нажимаем Continue to Hosted HOSTVM Manager Deployment

<figure><img src="https://lh3.googleusercontent.com/Fw0p8Yn55r3vRBu4iv53Ne8bWSTgNPweQFQ9b0xvsJN6fUjtqL6wR3U6xxxb__Xsb3v0NGaf2mQedADSsFGhwlSqtCSJ4nJ4jvG-kNcZHezC1Xs8zW0yCzY7_Q8T7_PIaDHGx1MTqeRxXx4NZpPhigo" alt=""><figcaption></figcaption></figure>

Заполните форму. Виртуальная машина создается со статическим файлом с использованием файла hosts.

<figure><img src="https://lh6.googleusercontent.com/BzOd1HFJnluY3VT1JGPCWMbqjQw0rjKzRrbeLU5Ydh6z7vZcC3lVk7D2xosllW2ZpsRdnK84RdqNEcs3hsuq6xBrZ7wN97qpOHmxGUGuk-w1oMR70zKXG-sojwOx97Huo-0Kun2zxS6IEvEtIjLn26w" alt=""><figcaption></figcaption></figure>

Заполните следующую форму. Укажите пароль от веб-интерфейса будущей виртуальной машины. Заполните настройки отправки уведомлений e-mail при необходимости.

<figure><img src="https://lh6.googleusercontent.com/9SQku4mRqjDvU7chUuRbZ6p3ycvfH-8hs9Mj-dYmPYbQZWzkTLJL1a6ggvSKZc5LOsBnoyhh70CrJf4wtkD9feX0XwUc7J87sWIgxA0I5GNLINommucu6pJKsqZHjgyZLaVc5KTXZdR_MTCHyPQo594" alt=""><figcaption></figcaption></figure>

Проверяем параметры, нажимаем Prepare VM

<figure><img src="https://lh5.googleusercontent.com/09_EJ5UQbJ3HILO7qWuMnTON3HDd7X3t4QJWXO52jt5pQy3u48WqlwLUHY7j3mmcUkH0zs6z0dAJ1MFGieo6PERPMFlBT6IwsE9TYIXo9D-5TaDqxZyt0gFg4o3w01eFEvc6fQZ7TYTzOpcJto4KukM" alt=""><figcaption></figcaption></figure>

\
Дожидаемся подготовки ВМ:

<figure><img src="https://lh4.googleusercontent.com/gYLTMeGNDEbeDwP09hdQOag9F5oZg8o2iFoRJSRmLCrwzMapnfvwcDfZRWsC9Ibrln8-Midk6yuK21IE6jGYg8K6AwH0PrR9EGUACbkGejwbLyz0LSJG6T1NcOxMqNo_j1SzMdMzeU81K_22dLNcJao" alt=""><figcaption></figcaption></figure>

Выбираем Storage Type = Gluster

Storage Connection = test-454:/engine

\*test-454 – имя ноды

\*engine – местонахождение хранилища

<figure><img src="https://lh3.googleusercontent.com/aDnjICfGB5iVv0Gb3JjSrf5s8Ezm1ZYC9xlq1rPuIKhwJF8J0fzcD2bKDtv4xUoEmjQxcvxhtH6H15ybwlG5GbCZd6PHAu4htgGNH_xJ_-mBHNhbqVKKNGLktHb3RcfZ5TDkfPau9LW_TOfd8mJqTx4" alt=""><figcaption></figcaption></figure>

Нажимаем Finish Deployment

<figure><img src="https://lh6.googleusercontent.com/9lGiAN2rID5sPwtGThjfH0JKbduNiNYf2JUI3frbd4xY-UB9wORHLfgA45seu-n8WzMIt1aPqGH84Y3houQ3Qr6vY0opcxDwYKN1Cb8RUKKwtxkonYrXYhsgzpssJfGn3IDUYz5gVnUb9fRY_lF6A_Q" alt=""><figcaption></figcaption></figure>

\


<figure><img src="https://lh4.googleusercontent.com/exhaUZHjVNgjR6wU2BBUg5U6mq6gNpueu8HrxYJW95FmheE7fvuF9olz9k66BtmTMnCgpvIFHTTjyfXk3IAbTTEu15ig5lVOYYh2YGBxhpI1SCqy9Hnpx2wm5JsYQ6v8ZUByuEuDID6dc1EfxAYuLI0" alt=""><figcaption></figcaption></figure>
