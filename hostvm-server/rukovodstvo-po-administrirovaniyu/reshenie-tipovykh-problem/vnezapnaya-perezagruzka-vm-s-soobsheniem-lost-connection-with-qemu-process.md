# Внезапная перезагрузка ВМ с сообщением "Lost connection with qemu process"

### Проблема

ВМ была внезапно перезагружена/приостановлена на хосте HOSTVM Node 4.4.8.&#x20;

В логах управляющей машины `engine.log`  присутствуют ошибки вида:

```
2025-12-15 20:51:11,759+11 ERROR [org.ovirt.engine.core.dal.dbbroker.auditloghandling.AuditLogDirector] 
(ForkJoinPool-1-worker-29) [4d2213d2] EVENT_ID: VM_DOWN_ERROR(119), VM yuzdc1-n-v70173 is down with error. 
Exit message: Lost connection with qemu process.
```

В логах хоста `/var/log/libvirt/qemu/<VM-name>.log`, на котором была запущена машина присутствуют ошибки вида:

```
  :
KVM: entry failed, hardware error 0x80000021
reason=crashed
  :
EAX=0b06e7a0 EBX=00000001 ECX=00000000 EDX=00000040
ESI=00000000 EDI=9cbd2180 EBP=8163ef50 ESP=8163eed0
EIP=00008000 EFL=00000002 [-------] CPL=0 II=0 A20=1 SMM=1 HLT=0
```

Значения в общих регистрах (EAX, EBX, ..., EFL) могут отличаться от приведенных в этом примере.

### Решение

Исправление этой ошибки присутствует в версиях ядра kernel-4.18.0-425.19.2.el8\_7 и выше.

* В HOSTVM 4.4.8 используется версия ядра 4.18.0-338.
* В HOSTVM 4.4.5 используется версия ядра 4.18.0-526

{% hint style="info" %}
Обновление версии ядра HOSTVM происходит синхронно с изменением версии ПО виртуализации. В рамках релиза версия ядра не изменяется.
{% endhint %}

Для решения проблемы необходимо выполнить обновление системы виртуализации HOSTVM до версии 4.5.5, согласно [процедуре обновления](../../installation-guide/obnovlenie-na-versiyu-4.5/).
