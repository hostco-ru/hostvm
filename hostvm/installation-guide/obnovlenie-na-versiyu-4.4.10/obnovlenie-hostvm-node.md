# Обновление HOSTVM Node

#### Требования для обновления

* Проверить что оборудование на обновляемом сервере поддерживается CentOS 8.x (HOSTVM Node 4.4.10) ([список неподдерживаемого оборудования](https://access.redhat.com/documentation/en-us/red\_hat\_enterprise\_linux/8/html/considerations\_in\_adopting\_rhel\_8/hardware-enablement\_considerations-in-adopting-rhel-8#removed-adapters\_hardware-enablement));
* Смигрировать все ВМ, запущенные на обновляемом сервере.

#### **Процесс обновления**

1\) Скачать iso-образ hostvm-node-ng-installer-4.4.10 из [Личного кабинета](https://lk.pvhostvm.ru/)

2\) Мигрировать с обновляемого хоста все ВМ на другие хосты кластера

3\) Перевести обновляемый хост в режим обслуживания через портал администрирования, затем удалить из кластера:

`Compute -> Hosts -> Выделить обновляемую ноду -> Management -> Maintenance`

`Compute -> Hosts -> Выделить обновляемую ноду -> Remove`

4\) Перезапустить сервер и загрузиться с ISO-образа hostvm-node-ng-installer-4.4.10&#x20;

5\) Выполнить установку согласно разделу "Процесс установки", на [локальные диски](../ustanovka-hostvm-cherez-cli-4.4-4.3/installation-hostvm-on-local-disks.md#process-ustanovki) либо [СХД](../ustanovka-hostvm-cherez-cli-4.4-4.3/installation-hostvm.md#process-ustanovki) (устанавливать hosted engine не требуется)&#x20;

6\) После успешной установки и перезапуска сервера на новой ОС, добавить хост в кластер через портал администрирования, указав hostname/IP-адрес:

`Compute -> Hosts -> New`

> Если требуется что бы хост мог обслуживать ВМ Hosted HOSTVM Manager, при добавлении в кластер также необходимо на вкладке Hosted Engine выбрать из списка Action: Deploy&#x20;

7\) Дождаться добавления и активации хоста в кластере&#x20;

8\) Установка закончена, при необходимости можно мигрировать ВМ обратно на обновленный хост
