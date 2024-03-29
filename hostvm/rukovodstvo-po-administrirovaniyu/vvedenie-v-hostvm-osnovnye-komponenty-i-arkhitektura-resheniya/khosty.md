# Хосты

Хосты (гипервизоры) – физические серверы, на которых работают виртуальные машины. Полная виртуализация обеспечивается загружаемым модулем ядра Linux: Kernel-based Virtual Machine (KVM). Он позволяет запускать множество ВМ с различными гостевыми ОС. ВМ работают как отдельные процессы Linux и управляются удаленно менеджером виртуализации HOSTVM.

Виртуализация HOSTVM поддерживает два типа ОС хоста: HOSTVM Node и Linux-based хост. В зависимости от требований может быть использован один тип или оба.

HOSTVM Node это сборка Linux-based ОС, включающая только необходимые компоненты для работы в качестве хоста виртуализации. Это упрощает управление, обслуживание и развертывание в среде HOSTVM. Дистрибутив доступен в виде ISO файла для установки на физические серверы. Включает в себя графический веб-интерфейс для администрирования сервера (Cockpit).

Рекомендуется развертывать как минимум два хоста в среде виртуализации HOSTVM. При подключении одного хоста не будут доступны функции «миграция» и «высокая доступность».

На всех хостах виртуализации работает агент управления VDSM (Virtual Desktop Server Manager), обеспечивающий связь между менеджером и хостами. VDSM позволяет менеджеру виртуализации HOSTVM управлять виртуальными машинами (в том числе их жизненным циклом), хранилищами данных, получать статистику от хостов и гостевых ОС.
