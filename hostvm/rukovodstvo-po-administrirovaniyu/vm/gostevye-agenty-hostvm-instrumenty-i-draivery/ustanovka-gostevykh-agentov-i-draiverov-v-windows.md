# Установка гостевых агентов и драйверов в Windows

Чтобы установить гостевые агенты, инструменты и драйверы на ВМ Windows, необходимо выполнить следующее:

1\.     На HOSTVM Manager установить пакет virtio-win: dnf install virtio-win\*

После установки пакета файл ISO будет находиться по пути _/usr/share/virtio-win/virtio-win-\<version>.iso_;

2\.     Загрузить virtio-win-\<version>.iso в домен хранения данных;

3\.     Если ВМ запущена, на портале администрирования или портале ВМ нажать кнопку «Сменить CD» («Change CD»), чтобы прикрепить файл virtio-win-\<version>.iso к ВМ. Если ВМ выключена, следует нажать кнопку «Выполнить один раз» («Run Once») и прикрепить ISO-образ как CD;

4\.     Авторизоваться в ВМ;

5\.     Выбрать CD-привод, содержащий файл virtio-win-\<version>.iso. Завершить установку можно с помощью графического интерфейса или командной строки, выполнив одно из следующих далее действий;

6\.     Для установки с помощью графического интерфейса:

●       двойным нажатием открыть файл virtio-win-gt-x64.msi или virtio-win-gt-x86.msi.;

●       нажать кнопку «Далее» («Next») на экране приветствия;

●       следовать подсказкам мастера установки. Убедиться, что в списке компонентов установлены все флажки, в том числе Guest-агент, который по умолчанию отключен;

●       после завершения загрузки выбрать «Да, я хочу перезагрузить компьютер сейчас» («Yes, I want to restart my computer now») и нажать «Готово» («Finish»), чтобы применить изменения;

●       после перезагрузки ВМ открыть CD-привод, содержащий файл virtio-win-\<version>.iso, перейти в каталог guest-agent и двойным нажатием открыть qemu-ga-x86\_64.msi или qemu-ga-i386.msi, чтобы установить qemu-ga, гостевой агент Qemu;

7\.     Для установки с помощью командной строки:

●       открыть командную строку с правами администратора;

●       ввести команду msiexec:\
_D:\ msiexec /i "PATH\_TO\_MSI" /qn \[/l\*v "PATH\_TO\_LOG"]\[/norestart] ADDLOCAL=ALL_

Например, чтобы запустить установку без сохранения журнала, когда virtio-win-gt-x64.msi находится на диске D:\\, а затем немедленно перезапустить ВМ, необходимо ввести следующую команду:\
_D:\ msiexec / i " virtio-win-gt-x64.msi "/ qn ADDLOCAL = ALL_

После завершения установки гостевые агенты и драйверы будут передавать информацию об утилизации ресурсов ВМ в HOSTVM Manager и позволят получать доступ к USB-устройствам, единому входу в ВМ и другим функциям. Гостевой агент работает как служба под названием oVirt Guest Service, которую можно настроить с помощью файла конфигурации ovirt-guest-agent.ini, расположенного по адресу: _C:\Program Files (x86)\oVirt Guest Tools_.