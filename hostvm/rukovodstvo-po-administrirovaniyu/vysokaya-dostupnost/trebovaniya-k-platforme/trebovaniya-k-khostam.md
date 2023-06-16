# Требования к хостам

HOSTVM поддерживает два типа ОС хоста: HOSTVM Node и Linux-based хост. В зависимости от требований может быть использован один тип или оба. Рекомендуемым вариантом ОС для установки на хосты является HOSTVM Node:

* в состав HOSTVM Node включены только необходимые компоненты для работы в качестве хоста виртуализации. Такой подход позволяет уменьшить накладные расходы ОС на виртуализацию (overhead). Как дополнительное преимущество, уменьшается количество возможных векторов атаки на систему за счет заранее заданных  ограничений в конфигурации по умолчанию;
* HOSTVM Node по умолчанию имеет рекомендуемую конфигурацию для использования в качестве хоста виртуализации и не требует ручной настройки. Такой подход позволяет устранить вероятность ошибок, связанных с ручной настройкой системы;
* HOSTVM Node включает в себя графический веб-интерфейс для администрирования сервера (Cockpit). Этот инструмент облегчает поиск и устранение проблем, связанных с хостом и запущенными на нем ВМ.

Конфигурация хоста виртуализации также должна включать:

* наличие внешнего независимого управления хостом (out-of-band (OOB) management, lights-out management (LOM)) для активации таких функций, как удаленное управление питанием;
* актуальные версии прошивок и BIOS;
* количество оперативной памяти, позволяющее избежать использование swap, которое существенно снижает производительность ВМ;
* настройку RAID для локальных загрузочных дисков, в случае их использования, для уменьшения вероятности отключения ВМ в случае отказа хоста.

Настройки управления питанием и изоляции хоста подробно рассматриваются в следующем разделе курса.