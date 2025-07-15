# Проблема с выпадающими списками

Если при использовании RemoteApp возникает проблема с выпадающими списками (мерцают, не раскрываются), то включите параметр "Запустить как оболочку" в настройках транспорта.

<figure><img src="../../../../.gitbook/assets/image (109).png" alt=""><figcaption></figcaption></figure>

Для брокера версии ниже HOSTVM-VDI3.6-20250314 воспользуйтесь запуском приложения через /shell,  указав в настройках транспорта Linux Client/Custom parameters:&#x20;`/shell:"||alias"`

<figure><img src="../../../../.gitbook/assets/image (106).png" alt=""><figcaption><p>Пример использования /shell</p></figcaption></figure>
