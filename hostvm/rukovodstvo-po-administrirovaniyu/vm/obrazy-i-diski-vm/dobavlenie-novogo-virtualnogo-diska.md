# Добавление нового виртуального диска

Образ – это тип диска по умолчанию. Также можно добавить диск Direct LUN или диск Cinder (том OpenStack). Создание образа диска полностью управляется менеджером виртуализации. Для дисков Direct LUN требуются уже существующие цели (target), подготовленные извне. Для дисков Cinder требуется доступ к экземпляру тома OpenStack, который был добавлен в среду HOSTVM с помощью интерфейса внешних поставщиков.

Существующие диски – это плавающие или общие диски, подключенные к ВМ.

Плавающие (floating) диски – это существующие диски, которые выделены какой-либо ВМ.

Для добавления диска на ВМ необходимо:

1. Перейти «Compute» -> «Virtual Machines»;
2. &#x20;Нажать на имя ВМ, чтобы перейти к просмотру сведений;
3. Перейти на вкладку «Disks»;
4. Нажать «New»;
5. Использовать соответствующие переключатели для перехода между «Image», «Direct LUN» или «Cinder»;
6. Ввести размер («Size») в ГБ, алиас («Alias») и описание («Description») для нового диска;
7. Использовать раскрывающиеся списки и флажки для настройки диска;
8. Нажать «OK».

Новый диск появится в подробном обзоре через короткое время.