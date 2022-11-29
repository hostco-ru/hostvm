# Table of contents

* [Документация HOSTVM](README.md)

## HOSTVM

* [Руководство по установке и настройке](hostvm/installation-guide/README.md)
  * [О платформе](hostvm/installation-guide/datasheet.md)
  * [Системные требования](hostvm/installation-guide/requirements.md)
  * [Базовые условия для развертывания платформы виртуализации HOSTVM](hostvm/installation-guide/bazovye-usloviya-dlya-razvertyvaniya-platformy-virtualizacii-hostvm.md)
  * [Установка HOSTVM через GUI 4.4](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/README.md)
    * [Перед установкой](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/pered-ustanovkoi.md)
    * [Процесс установки](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/process-ustanovki.md)
    * [Установка HOSTVM Manager на FC-диск](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-fc-disk.md)
    * [Установка HOSTVM Manager на NFS](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-nfs.md)
    * [Установка HOSTVM Manager на Gluster](hostvm/installation-guide/ustanovka-hostvm-cherez-gui-4.4/ustanovka-hostvm-manager-na-gluster.md)
  * [Установка HOSTVM через CLI (4.4, 4.3)](hostvm/installation-guide/ustanovka-hostvm-cherez-cli-4.4-4.3/README.md)
    * [Установка HOSTVM Node на локальные диски](hostvm/installation-guide/ustanovka-hostvm-cherez-cli-4.4-4.3/installation-hostvm-on-local-disks.md)
    * [Установка HOSTVM Node на диски СХД (glusterfs)](hostvm/installation-guide/ustanovka-hostvm-cherez-cli-4.4-4.3/installation-hostvm.md)
    * [Установка консоли (веб-интерфейса, HostedEngine) управления на СХД](hostvm/installation-guide/ustanovka-hostvm-cherez-cli-4.4-4.3/ustanovka-konsoli-veb-interfeisa-hostedengine-upravleniya-na-skhd.md)
  * [Действия после установки виртуализации](hostvm/installation-guide/after-install.md)
  * [Конвертация, импорт виртуальных машин](hostvm/installation-guide/vm-import/README.md)
    * [Конвертация ВМ из VMware](hostvm/installation-guide/vm-import/import-from-vmware/README.md)
      * [Конвертация виртуальной машины с ОС AltLinux из VMware в HOSTVM](hostvm/installation-guide/vm-import/import-from-vmware/vmware\_convert\_altlinux.md)
    * [Конвертация ВМ из Microsoft Hyper-V](hostvm/installation-guide/vm-import/import-from-hyperv.md)
    * [Предварительная установка драйверов](hostvm/installation-guide/vm-import/drivers-preinstallation.md)
  * [Установка Kaspersky Security для виртуальных сред 5.х](hostvm/installation-guide/installation-ksc.md)
  * [Установка Accord KVM](hostvm/installation-guide/installation-accordkvm.md)
  * [Обновление на версию 4.4.10](hostvm/installation-guide/obnovlenie-na-versiyu-4.4.10/README.md)
    * [Перечень изменений версии 4.4.10](hostvm/installation-guide/obnovlenie-na-versiyu-4.4.10/perechen-izmenenii-versii-4.4.10.md)
    * [Обновление Hosted HOSTVM Manager](hostvm/installation-guide/obnovlenie-na-versiyu-4.4.10/obnovlenie-hosted-hostvm-manager.md)
    * [Обновление HOSTVM Node](hostvm/installation-guide/obnovlenie-na-versiyu-4.4.10/obnovlenie-hostvm-node.md)
  * [Обновление на версию 4.4](hostvm/installation-guide/obnovlenie-na-versiyu-4.4/README.md)
    * [Обновление Hosted HOSTVM Manager](hostvm/installation-guide/obnovlenie-na-versiyu-4.4/obnovlenie-hosted-engine.md)
    * [Обновление HOSTVM Node](hostvm/installation-guide/obnovlenie-na-versiyu-4.4/obnovlenie-hostvm-node.md)
  * [Действия при установке HOSTVM при отсутствии записей в DNS](hostvm/installation-guide/deistviya-pri-ustanovke-hostvm-pri-otsutstvii-zapisei-v-dns.md)

## HOSTVM VDI

* [Руководство по установке и настройке](hostvm-vdi/hostvm-vdi-installation-guide/README.md)
  * [Системные требования](hostvm-vdi/hostvm-vdi-installation-guide/requirements.md)
  * [Установка HOSTVM VDI Broker](hostvm-vdi/hostvm-vdi-installation-guide/hostvm-vdi-ova-install.md)
  * [Установка HOSTVM VDI Tunneler](hostvm-vdi/hostvm-vdi-installation-guide/tunneler-appliance-deploy.md)
  * [Настройка USB-загрузчика для клиентских устройств](hostvm-vdi/hostvm-vdi-installation-guide/usb-boot-device-configuration.md)
  * [Обновление компонентов VDI](hostvm-vdi/hostvm-vdi-installation-guide/obnovlenie-komponentov-vdi.md)
  * [Обновление конфигурации из двух экземпляров брокера VDI со встроенной БД (с репликацией)](hostvm-vdi/hostvm-vdi-installation-guide/obnovlenie-konfiguracii-iz-dvukh-ekzemplyarov-brokera-vdi-so-vstroennoi-bd-s-replikaciei.md)
  * [Изменения и исправления](hostvm-vdi/hostvm-vdi-installation-guide/izmeneniya-i-ispravleniya.md)
* [Руководство администратора](hostvm-vdi/hostvm-vdi-admin-guide/README.md)
  * [Подготовка  базового образа для публикации](hostvm-vdi/hostvm-vdi-admin-guide/base-image-preparation/README.md)
    * [Установка HOSTVM VDI Actor для Astra Linux](hostvm-vdi/hostvm-vdi-admin-guide/base-image-preparation/ustanovka-hostvm-vdi-actor-dlya-astra-linux.md)
    * [Ввод тонких клонов в домен для Astra Linux](hostvm-vdi/hostvm-vdi-admin-guide/base-image-preparation/vvod-tonkikh-klonov-v-domen-dlya-astra-linux.md)
  * [Настройка сервис-провайдеров](hostvm-vdi/hostvm-vdi-admin-guide/service-providers/README.md)
    * [Настройка сервиса на основе провайдера HOSTVM](hostvm-vdi/hostvm-vdi-admin-guide/service-providers/nastroika-servisa-na-osnove-provaidera-hostvm.md)
  * [Управление методами аутентификации](hostvm-vdi/hostvm-vdi-admin-guide/authenticators/README.md)
    * [Radius](hostvm-vdi/hostvm-vdi-admin-guide/authenticators/radius.md)
    * [SAML](hostvm-vdi/hostvm-vdi-admin-guide/authenticators/saml.md)
  * [Настройка менеджеров ОС](hostvm-vdi/hostvm-vdi-admin-guide/os-managers.md)
  * [Настройка транспорта подключения](hostvm-vdi/hostvm-vdi-admin-guide/transports.md)
  * [Настройка пула сервисов](hostvm-vdi/hostvm-vdi-admin-guide/service-pools.md)
  * [Терминальные серверы и приложения](hostvm-vdi/hostvm-vdi-admin-guide/terminal-servers-and-apps/README.md)
    * [Настройка подключений по протоколу x2Go](hostvm-vdi/hostvm-vdi-admin-guide/terminal-servers-and-apps/nastroika-podklyuchenii-po-protokolu-x2go.md)
    * [Настройка подключений по протоколу RemoteApp](hostvm-vdi/hostvm-vdi-admin-guide/terminal-servers-and-apps/nastroika-podklyuchenii-po-protokolu-remoteapp.md)
  * [Установка и настройка выделенного сервера БД](hostvm-vdi/hostvm-vdi-admin-guide/vdi-db.md)
  * [Настройка репликации БД между серверами](hostvm-vdi/hostvm-vdi-admin-guide/vdi-db-replication.md)
* [Руководство пользователя](hostvm-vdi/hostvm-vdi-user-manual.md)

## HOSTVM SDS

* [Руководство по установке и настройке](hostvm-sds/guide/README.md)
  * [Создание хранилища через CLI](hostvm-sds/guide/cli-new-storage.md)
  * [Расширение тома Hosted-engine до Replica 3](hostvm-sds/guide/rasshirenie-toma-hosted-engine-do-replica-3.md)

## HOSTVM Backup

* [Руководство по установке и настройке](hostvm-backup/rukovodstvo-po-ustanovke-i-nastroike/README.md)
  * [Импорт и первичная настройка](hostvm-backup/rukovodstvo-po-ustanovke-i-nastroike/import-i-pervichnaya-nastroika.md)

## О Компании

* [Команда разработчиков HOSTVM](o-kompanii/developer.md)
