# Проблема с запуском вложенных приложений

При использовании /shell в RemoteApp не открываются вложенные приложения со следующей ошибкой:

> Приложение не найдено:> \
> C:\Windows\system32\start.exe

Данная  ошибка связана с некорректным расположением .exe файла, для ее исправления укажите правильную рабочую директорию в настройках транспорта Linux Client/Custom parameters:\
&#x20;`/shell-dir:"C:\Path\To"`

<figure><img src="../../../../.gitbook/assets/image (110).png" alt=""><figcaption><p>Пример использования /shell-dir</p></figcaption></figure>

