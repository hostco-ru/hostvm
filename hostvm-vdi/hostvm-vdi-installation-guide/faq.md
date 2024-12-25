# Часто задаваемые вопросы

### **Известные ошибки при импорте**

Если при попытке подключения консолью появляется сообщение "Connected to graphic server":

<figure><img src="../../.gitbook/assets/photo_2023-04-26_17-00-54.jpg" alt=""><figcaption><p>Ошибка "Connected to graphic server"</p></figcaption></figure>

в качестве решения необходимо проверить значение свойства Chipset/Firmware Type.\
(Compute -> Virtual Machines -> Выбрать нужную ВМ -> Edit -> System).\
Если значение отлично от "Q35 Chipset with BIOS", необходимо привести его к этому состоянию:

<figure><img src="../../.gitbook/assets/example.jpg" alt=""><figcaption><p>Q35 Chipset with BIOS</p></figcaption></figure>

### Импорт апплаенса в VMware

Формат апплаенсов HOSTVM VDI не поддерживается со стороны VMware.

Возможна конвертация дисков в формат vmdk с помощью утилиты qemu-img (есть в составе Linux-подобных операционных систем), и импорт их в VMWare. Для этого распакуйте .ova файл брокера:&#x20;

```
tar xvf hostvm-vdi.ova
```

На выходе будет 2 файла: vm.ovf и диск ВМ (например c9bfb5c7-edc0-4596-adca-f63cca42b4c9)

Конвертируйте диск командой:&#x20;

```
qemu-img convert -f qcow2 c9bfb5c7-edc0-4596-adca-f63cca42b4c9 -O vmdk hostvm-vdi.vmdk
```

После выполнения команды вы получите файл vmdk для VMware Workstation. Получившийся vmdk необходимо еще раз конвертировать на хосте esxi:&#x20;

```
vmkfstools -i source.vmdk -d zeroedthick converted_source.vmdk  
```

Поскольку xml-файл с настройками машины не импортируется, необходимо выставить их вручную в настройках ВМ после импорта.

{% hint style="warning" %}
**Для конвертации vmdk имеются следующие ограничения:**&#x20;

VMDK disks converted through qemu-img are always monolithic sparse VMDK disks with an IDE adapter type.
{% endhint %}

### Скрытие панели уведомления о использовании cookie



<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

В случае, когда необходимо скрыть всплывающую панель с уведомлением о использовании cookie необходимо выполнить следующие действия:

\
На каждом брокере, где необходимо внести изменения:\
1\. Сделать резервную копию файла _**scripts.js**_

```
cp /var/server/static/modern/scripts.js /var/server/static/modern/scripts_bkp.js
```

2\. Отредактировать файл _**scripts.js**_, заменив

{% code overflow="wrap" %}
```
window:'<div role="dialog" aria-live="polite" aria-label="cookieconsent" aria-describedby="cookieconsent:desc" class="cc-window {{classes}}">\x3c!--googleoff: all--\x3e{{children}}\x3c!--googleon: all--\x3e </div> '
```
{% endcode %}

следующим содержимым

```
window:'<div> </div>'
```

### Запрет запуска более одной копии RemoteApp и HTML5 RemoteApp приложения для каждого пользователя

В случае, когда необходимо запретить пользователям вызов более чем одной копии приложения, необходимо добавить batch скрипт-прослойку на каждый сервер с ролью RDSH.&#x20;

При такой конфигурации необходимо презентовать в качестве приложения коллекции сам скрипт, а не оригинальное приложение.

* Если необходимо запретить использование более чем одной копии, независимо от параметров запуска, можно ориентироваться на следующий пример:

```
@echo off
setlocal

rem Получаем имя текущего пользователя
for /f "tokens=1" %%u in ('whoami') do set USERNAME=%%u

rem Проверяем, запущен ли калькулятор текущим пользователем
tasklist /FI "USERNAME eq %USERNAME%" | findstr "win32calc.exe" >nul
if %errorlevel% equ 0 goto running

rem Если калькулятор не запущен, запускаем его
start " " "%windir%\system32\win32calc.exe"
exit

:running
exit
```

* Если необходимо ограничивать запуск копии приложения, ориентируясь на параметры запуска (то есть допускать запуск пользователем одного и того же приложения, в случае если параметры запуска различаются) , можно ориентироваться на следующий пример:

```
@echo off
chcp 1251
setlocal EnableDelayedExpansion

set "app=notepad.exe"
set "args=C:\Windows\System32\NOTEPAD.EXE C:\test\test.txt"

echo App: !app!
echo Args: !args!

set "nospArgs=!args: =!"
echo Параметры без пробелов: !nospArgs!

echo Получаем имя текущего пользователя
for /f "tokens=1" %%u in ('whoami') do set USERNAME=%%u

echo Получаем ID процесса !app! из tasklist
set tasklistId=
for /f "tokens=2" %%p in ('tasklist /FI "USERNAME eq %USERNAME%" ^| findstr "!app!"') do (
    set tasklistId=%%p
)

echo Проверяем, найден ли процесс
if "%tasklistId%"=="" (
    echo Процесс !app! не найден в tasklist.
    goto :LAUNCH
    pause
    exit /b
) ELSE (
echo task: !tasklistId!

set found=
echo Получаем вывод wmic
for /f "skip=1" %%i in ('wmic process where "name='!app!'" get ProcessId') do (
    set "wmicId=%%i"
    set "wmicId=!wmicId: =!"
    echo wmic: !wmicId! 
    echo ----------
    if "!wmicId!"=="!tasklistId!" (
        echo ID процесса !wmicId! найден в списке ID из wmicId.
  set "par="
  for /f "skip=1 tokens=* delims=" %%i in ('wmic process where "processid='!wmicId!'" get commandline') do (
          set "par=%%i"
        rem Выход из цикла после первого найденного значения
        if defined par (
          goto :break
    )
)
  :break
        echo Совпадение id обнаружено.
        echo Обнаруженные параметры запуска приложения: !par!
        rem Удаляем пробелы из параметров запуска
  set "nospPar=!par: =!"

  rem Проверяем длины
  set "lenNospPar=0"
  for /l %%i in (0,1,100) do if "!nospPar:~%%i,1!" neq "" set /a lenNospPar+=1
  echo Длина nospPar: !lenNospPar!

  set "lenNospArgs=0"
  for /l %%i in (0,1,100) do if "!nospArgs:~%%i,1!" neq "" set /a lenNospArgs+=1
  echo Длина nospArgs: !lenNospArgs!

  if !lenNospPar! equ !lenNospArgs! (
      echo Длины равны
  ) else if !lenNospArgs! equ !lenNospPar! + 1 (
      echo Пробуем удалить последний символ
      set "nospPar=!nospPar:~0,-1!"
    )

    if "!nospPar!"=="!nospArgs!" (
    echo Параметры совпадают. Приложение не требует запуска.
    set "found=1"
    goto :running
) else (
    echo nospPars: !nospPar!
    echo nospArgs: !nospArgs!
    echo Параметры не совпадают. Пропускаем.
)
    )
)

if not defined found (
    echo Процесс !app! с параметрами запуска !args! не найден в выводе wmic 
    :LAUNCH
    echo Запускаем 
    start " " %args%
)
:running
)
```
