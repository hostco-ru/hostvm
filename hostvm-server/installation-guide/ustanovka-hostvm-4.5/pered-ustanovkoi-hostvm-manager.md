# Перед установкой HOSTVM Manager

В случае, если установка происходит с использованием локального репозитория (hostvm-node-ng-installer-local-repo), то после подключения этого диска нужно перезагрузить сервис systemd-udevd:

```
systemctl restart systemd-udevd
```

Остальные подготовительные мероприятия аналогичны версии 4.3, 4.4:\
[Заполнение формы для установки значений переменных](../ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/zapolnenie-formy-dlya-ustanovki-znachenii-peremennykh.md)\
[Подготовка putty к работе](../ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/podgotovka-putty-k-rabote.md)\
[Подготовка NFS share](../ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/podgotovka-nfs-share.md)\
[Подготовка multipath](../ustanovka-hostvm-4.3-4.4/pered-ustanovkoi-hostvm-manager/podgotovka-multipath.md)

