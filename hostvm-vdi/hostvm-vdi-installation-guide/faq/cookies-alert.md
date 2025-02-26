# Скрытие панели уведомления об использовании cookies

В случае, когда необходимо скрыть всплывающую панель с уведомлением о использовании cookie

<figure><img src="../../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Необходимо выполнить следующие действия:

На каждом брокере, где необходимо внести изменения:

1\. Сделать резервную копию файла _**scripts.js**_

```
cp /var/server/static/modern/scripts.js /var/server/static/modern/scripts_bkp.js
```

2\. Отредактировать файл _**scripts.js**_, заменив

```
window:'<div role="dialog" aria-live="polite" aria-label="cookieconsent" aria-describedby="cookieconsent:desc" class="cc-window {{classes}}">\x3c!--googleoff: all--\x3e{{children}}\x3c!--googleon: all--\x3e </div> '
```

следующим содержимым

```
window:'<div> </div>'
```
