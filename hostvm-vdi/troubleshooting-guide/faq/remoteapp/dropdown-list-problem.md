# Проблема с выпадающими списками

Если при использовании RemoteApp возникает проблема с выпадающими списками (мерцают, не раскрываются), то включите параметр "Запустить как оболочку" в настройках транспорта.

<figure><img src="https://raw.githubusercontent.com/hostco-ru/hostvm/master/.gitbook/assets/dropdown-list-problem1.png" alt=""><figcaption></figcaption></figure>

Для брокера версии ниже HOSTVM-VDI3.6-20250314 воспользуйтесь запуском приложения через /shell, указав в настройках транспорта Linux Client/Custom parameters: `/shell:"||alias"`

<figure><img src="https://raw.githubusercontent.com/hostco-ru/hostvm/master/.gitbook/assets/dropdown-list-problem2.png" alt=""><figcaption><p>Пример использования /shell</p></figcaption></figure>
