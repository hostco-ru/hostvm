# Процедура установки для версии HOSTVM 4.4

Чтобы установить оптимизированные для VirtIO драйверы дисков и сетевых устройств во время установки Windows, необходимо подключить образ virtio-wi&#x6E;_\_version_.iso к ВМ. Эти драйверы обеспечивают повышение производительности по сравнению с эмулируемыми драйверами устройств.

Необходимо использовать параметр «Выполнить один раз» («Run Once») при запуске ВМ, чтобы подключить образ для однократной загрузки, отличной от параметров загрузки, определенных в окне «Новая ВМ» («New Virtual Machine»). В этой процедуре предполагается добавление сетевого интерфейса HOSTVM VirtIO и диска, который использует интерфейс VirtIO к ВМ.

Для установки драйверов VirtIO при установке Windows выполните следующее:

1. Перейти «Compute» -> «Virtual Machines» и выбрать ВМ;
2. Перейти «Run» -> «Run Once»;
3. Развернуть меню параметров загрузки («Boot Options»);
4. Установить флажок «Attach CD» и выбрать требуемый ISO-образ Windows из раскрывающегося списка;
5. Установить флажок «Attach Windows guest tools CD»;
6. Переместить CD-ROM в верхнюю часть поля «Boot Sequence»;
7. При необходимости настроить остальные параметры однократного запуска;
8. Нажать «OK». Состояние ВМ изменится на «Up», начнется установка ОС;
9. Открыть консоль ВМ, если она не открывается автоматически;
10. При запросе на выбор диска, на который будет производиться установка ОС, нажать Load driver и ОК;
11. В меню выбора драйвера указать соответствующий устанавливаемой версии ОС;
12. Нажать Next, далее следовать стандартному процессу установки ОС.
