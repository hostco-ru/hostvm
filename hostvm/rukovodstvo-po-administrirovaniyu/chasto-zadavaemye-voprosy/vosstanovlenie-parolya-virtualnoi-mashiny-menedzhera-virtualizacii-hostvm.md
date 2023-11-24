# Восстановление пароля виртуальной машины менеджера виртуализации HOSTVM

Переведите менеджер виртуализации в режим обслуживания

```
hosted-engine --set-maintenance --mode=global
```

Выключите ВМ менеджера виртуализации

```
hosted-engine --vm-shutdown
```

Запустите ВМ менеджера виртуализации в режиме паузы&#x20;

```
hosted-engine --vm-start-paused
```

Задайте пароль для VNC подключения

```
hosted-engine --add-console-password
```

```
Пример ожидаемого вывода: 
You can now connect the hosted-engine VM with VNC at 10.1.140.100:5902
```

Подключитесь по указанному адресу, например, `10.1.140.100:5902` через любой VNC клиент, например, используя TightVNC Viewer

Вывести ВМ менеджера виртуализации из паузы

```
virsh -c qemu:///system?authfile=/etc/ovirt-hosted-engine/virsh_auth.conf resume HostedEngine
```

Быстро вернитесь в интерфейс клиента VNC и выбрав нужное ядро нажать клавишу `'e'`. Вы будете переключены на экран, где необходимо  отредактировать скрипт загрузчика grub.

В строке, начинающейся с `linux` или `linux16`, в зависимости от версии, удалите все значения для console, например:&#x20;

<pre><code><strong>console=tty0 &#x26; console=ttyS0,115200n8
</strong></code></pre>

В конце этой же строки добавьте&#x20;

```
rd.break 
```

\
Пример внесения изменений:

<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>Скрипт до изменений</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>Скрипт с внесенными изменениями</p></figcaption></figure>

Запустите измененный скрипт загрузчика комбинаций клавиш `Ctrl+X`

Должна произойти загрузка в rescue режиме

```
switch_root:/#
```

Перемонтируйте раздел root командой&#x20;

```
mount -o remount rw /sysroot
```

Перейдите в директорию sysroot

```
chroot /sysroot
```

Смените пароль

<pre><code><strong>passwd
</strong></code></pre>

Передайте сигнал об изменениях для SELinux&#x20;

```
touch /.autorelabel
```

Завершите внесение изменений&#x20;

```
exit
```

```
reboot
```

Запустите ВМ менеджера виртуализации&#x20;

```
hosted-engine --vm-start
```

Выключите режим обслуживания

```
hosted-engine --set-maintenance --mode=none
```
