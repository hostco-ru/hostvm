# Откат изменений

При наличии ранее созданного снапшота можно выключить ВМ и перезапустить ее, используя этот снапшот. Есть возможность предпросмотра снапшота перед подтверждением отката ВМ на него. В этом режиме ВМ работает с использованием в качестве образа диска снапшот. Это позволяет убедиться в том, что выбран нужный снапшот. После проверки можно подтвердить откат изменений. При этом текущий образ диска вернется к состоянию в снапшоте, а все снапшоты, сделанные позже, будут удалены. Для предпросмотра снапшота и отката состояния ВМ через портал администрирования выполните следующие действия:

1. Перейдите в раздел Compute > Virtual Machines, выберите ВМ из списка и убедитесь, что она выключена;
2. Нажмите на ссылку в имени ВМ, чтобы открыть ее свойства, перейдите на вкладку Snapshots. Выберите из списка снапшот, который хотите восстановить;
3. Нажмите кнопку Preview. Если снапшот был сделан с сохранением состояния оперативной памяти, система дополнительно запросит подтверждение о необходимости восстановления памяти. Снапшот перейдет в статус In Preview, означающий, что он готов к запуску;
4. На этом этапе вы можете выполнить предпросмотр снапшота. Для этого необходимо просто запустить ВМ. После принятия решения, выполнять откат изменений или нет, выключите ВМ;
5. Для отката изменений нажмите кнопку Commit на вкладке Snapshots свойств машины. Это вернет состояние ВМ на момент снятия снапшота, а также удалит все снапшоты, сделанные позже него. После этого вы можете запустить ВМ. Если вы решили не откатывать изменения, нажмите кнопку Undo на вкладке Snapshots. Статус снапшота изменится с In Preview на OK, а текущий активный образ с Locked на OK. После этого вы можете запустить ВМ или попробовать откатиться на другой снапшот.

Решение об откате на конкретный снапшот необратимо. Текущее активное состояние и все снапшоты, сделанные позже того, на который был выполнен откат, включая все уникальные для этих снапшотов данные, будут безвозвратно потеряны.