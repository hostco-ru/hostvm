# Импорт и первичная настройка

**Минимальные системные требования для развертывания виртуальной машины:**

Оперативная память: 2048 MB

Количество виртуальных CPU: 1

Объем виртуального диска: 10 GB

#### **1. Размещение шаблона ВМ резервного копирования на хосте виртуализации:**

{% hint style="info" %}
**Примечание: для загрузки образа необходимо минимум 6 GB свободного места в разделе.**
{% endhint %}

Загрузите образ в директорию хоста виртуализации удобным способом:

```
mkdir /home/temp_backup
cd /home/temp_backup
```

#### **2. Импорт через веб-интерфейс HOSTVM:**

* _Compute->Virtual Machines_
* Активировать выпадающее меню (три точки, справа от кнопки Create Snapshot)_->Import_
* _Data Center ->_ выбрать нужный
* _Source -> Virtual Appliance (OVA)_
* _Host ->_ Выбрать нужный, на котором хранится \*.ova из пунктов выше
* _File Path ->_ /home/temp\_backup на примере пунктов выше.
* Нажать кнопку _Load_ (если на этом этапе ВМ не была обнаружена, необходимо изменить права доступа к \*.ova, или директорию его размещения)
* Выбрать _образ_ и переместить в раздел _Virtual Machines to Import_.
* Нажать кнопку _Next_
* Изменить _Storage Domain_ и пр, в случае необходимости.
* Нажать кнопку _OK_ (если на этом этапе возникли ошибки импорта, необходимо изменить права доступа к \*.ova или директорию его размещения)

#### **3. Первичная настройка HOSTVM Backup:**

Реквизиты для ВМ: root/backup

* Произвести конфигурацию сети:

```
ip a (уточняем имя сетевого адаптера)
cd /etc/sysconfig/network-scripts/
vi ifcfg-ens3 (укажите имя своего адаптера вместо ens3)
```

* Изменить значения строк:

```
BOOTPROTO=static
NAME=имя_сетевого_адаптера
UUID=закомментировать знаком решетки (#UUID)
DEVICE=имя_сетевого_адаптера
IPADDR=указать IP адрес
PREFIX=указать маску подсети
GATEWAY=указать адрес шлюза
DNS1=указать первичный DNS адрес
DNS2=указать вторичный DNS адрес
systemctl restart NetworkManager 
```

#### **4. Загрузка сертификата:**

Для проверки SSL сертификата, система резервного копирования должна иметь доступ к СА-сертификату целевого HOSTVM Engine, этот сертификат можно загрузить со стартовой страницы Engine либо c помощью следующей команды:

```
curl -k -o /etc/bareos/ovirt-ca.cert <https://engine.example.com/ovirt-engine/services/pki-resource?resource=ca-certificate&format=X509-PEM-CA>
```

#### **5. Создание задач резервного копирования:**

Для каждой виртуальной машины которой требуется резервное копирование необходимо сконфигурировать файлы job и fileset. Например, для резервного копирования виртуальной машины testvm1, сконфигурируйте fileset:

```
vi /etc/bareos/bareos-dir.d/fileset/testvm1_fileset.conf
```

Скопируйте в него пример конфигурации:

```
FileSet {
   Name = "testvm1_fileset"

   Include {
      Options {
         signature = MD5
         Compression = LZ4
      }
      Plugin = "python3"
               ":module_path=/usr/lib64/bareos/plugins"
               ":module_name=bareos-fd-ovirt"
               ":ca=/etc/bareos/ovirt-ca.cert"
               ":server=engine.example.com"
               ":username=admin@internal"
               ":password=secret"
               ":vm_name=testvm1"
   }
}
```

По аналогии создайте job со следующим содержимым:

```
vi /etc/bareos/bareos-dir.d/job/testvm1_job.conf
Job {
   Name = "testvm1_job"
   JobDefs = "DefaultJob"
   FileSet = "testvm1_fileset"
}
```

#### **6. Запуск задачи резервного копирования:**

```
bconsole
*reload
*run job=testvm1_job level=Full
```

#### **7. Запуск восстановления:**

```
bconsole
*restore
Select item:  (1-13): 5
Select the Client (1-5): //выберите нужный 
mark *
done
На вопрос "OK to run? (yes/mod/no)" введите mod
Select parameter to modify (1-14): 14
Plugin Options:  python3:storage_domain=YOUR_STORAGE_DOMAIN:vm_name=testvm1restore (для клонирования ВМ)
Plugin Options:  python3:storage_domain=YOUR_STORAGE_DOMAIN:vm_name=testvm1restore:override=yes (для перезаписи ВМ)
```
