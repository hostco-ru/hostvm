# Языковая панель в HTML5 сессии

В HTML5 сессии языковая панель не предусмотрена.

В случае, если языковая панель в HTML5 сессии необходима, возможно использование приложения HTML5LanguageBarApp (доступно в личном кабинете).&#x20;

<figure><img src="../../../.gitbook/assets/image (104).png" alt=""><figcaption><p>HTML5LanguageBarApp</p></figcaption></figure>

{% hint style="danger" %}
Для корректной работы приложения на клиентской машине должен быть выставлен английский язык.&#x20;
{% endhint %}

{% hint style="warning" %}
На терминальном сервере должны быть установлены необходимые языки.
{% endhint %}

#### Для работы потребуется:

1. на терминальный сервер из личного необходимо скачать архив HTML5LanguageBarApp.zip и извлечь файлы;
2. на терминальный сервер установить MS Widnows Desktop Runtime 7.0.20;
3. отредактировать файл `/var/server/uds/transports/HTML5RA/html5ra.py` на HOSTVM VDI Broker, добавив в секцию **"Build params dict"** параметр **allowed-languages** и указать необходимые языки:

<figure><img src="../../../.gitbook/assets/image (101).png" alt=""><figcaption></figcaption></figure>

#### Запуск приложения

Приложение запускается с помощью HTML5LanguageBar.exe.&#x20;

Для запуска  языковой панели в HTML5 сессии необходимо опубликовать приложение через \
bat-файл со следующим содержимым (на примере notepad):

```
@echo off
rem Полный путь к публикуемому приложению и параметры запуска приложения (при наличии)
start "" "C:\Windows\system32\notepad.exe" "C:\RDS\test1.txt"

rem Запрет запуска копии приложения HTML5LanguageBar 
setlocal 
for /f "tokens=1" %%u in ('whoami') do set USERNAME=%%u
 
tasklist /FI "USERNAME eq %USERNAME%" | findstr "HTML5LanguageBar.exe" >nul
if %errorlevel% equ 0 goto running

rem Полный путь к приложению HTML5LanguageBar   
start "" "C:\RDS\HTML5LanguageBarApp\HTML5LanguageBar.exe"
exit
 
:running
exit
```

#### Поддерживаемые языки:

<table><thead><tr><th>Keyboard layout</th><th>Parameter value</th><th data-hidden></th></tr></thead><tbody><tr><td>Brazilian (Portuguese)</td><td>pt-br-qwerty</td><td></td></tr><tr><td>English (UK)</td><td>en-gb-qwerty</td><td></td></tr><tr><td>English (US)</td><td>en-us-qwerty</td><td></td></tr><tr><td>French</td><td>fr-fr-azerty</td><td></td></tr><tr><td>French (Belgian)</td><td>fr-be-azerty</td><td></td></tr><tr><td>French (Swiss)</td><td>fr-ch-qwertz</td><td></td></tr><tr><td>German</td><td>de-de-qwertz</td><td></td></tr><tr><td>German (Swiss)</td><td>de-ch-qwertz</td><td></td></tr><tr><td>Hungarian</td><td>hu-hu-qwertz</td><td></td></tr><tr><td>Italian</td><td>it-it-qwerty</td><td></td></tr><tr><td>Japanese</td><td>ja-jp-qwerty</td><td></td></tr><tr><td>Norwegian</td><td>no-no-qwerty</td><td></td></tr><tr><td>Spanish</td><td>es-es-qwerty</td><td></td></tr><tr><td>Spanish (Latin American)</td><td>es-latam-qwerty</td><td></td></tr><tr><td>Sweedish</td><td>sv-se-qwerty</td><td></td></tr><tr><td>Turkish-Q</td><td>tr-tr-qwerty</td><td></td></tr></tbody></table>
