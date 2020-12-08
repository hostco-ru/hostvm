# Обновление HOSTVM Node

#### Требования для обновления

* Проверить что оборудование на обновляемом сервере поддерживается EL8 \([список неподдерживаемого оборудования](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/considerations_in_adopting_rhel_8/hardware-enablement_considerations-in-adopting-rhel-8#removed-adapters_hardware-enablement)\);
* Смигрировать все ВМ, запущенные на обновляемом сервере.

#### **Процесс обновления**

1\) Скачать iso-образ hostvm-node-ng-installer-4.4 из [каталога загрузок](https://reestr.hostco.ru/downloads)

2\) Мигрировать с обновляемого хоста все ВМ на другие хосты кластера

3\) Перевести обновляемый хост в режим обслуживания через портал администрирования, затем удалить из кластера:

`Compute -> Hosts -> Выделить обновляемую ноду -> Management -> Maintenance`

`Compute -> Hosts -> Выделить обновляемую ноду -> Remove`

4\) Перезапустить сервер и загрузиться с ISO-образа hostvm-node-ng-installer-4.4 

5\) Выполнить установку согласно разделу "Процесс установки", на [локальные диски](../installation-hostvm-on-local-disks.md#process-ustanovki) либо [СХД](../installation-hostvm.md#process-ustanovki) \(устанавливать hosted engine не требуется\) 

6\) После успешной установки и перезапуска сервера на новой ОС, добавить хост в кластер через портал администрирования, указав hostname/IP-адрес:

`Compute -> Hosts -> New`

> Если требуется что бы хост мог обслуживать ВМ Hosted Engine, при добавлении в кластер также необходимо на вкладке Hosted Engine выбрать из списка Action: Deploy

7\) Дождаться добавления и активации хоста в кластере 

8\) Установка закончена, при необходимости можно мигрировать ВМ обратно на обновленный хост

