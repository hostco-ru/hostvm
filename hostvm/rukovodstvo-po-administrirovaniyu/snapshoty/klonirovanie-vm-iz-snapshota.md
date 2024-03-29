# Клонирование ВМ из снапшота

Любой существующий снапшот может быть использован для клонирования ВМ. Клонирование из снапшота вместо актуального состояния машины может быть полезно, если необходимо сделать копию предыдущего состояния ВМ. Для клонирования ВМ из существующего снапшота, выполните следующие действия на портале администрирования:

1. Перейдите в раздел Compute > Virtual Machines, выберите ВМ из списка, нажмите на ссылку в ее имени, чтобы открыть свойства, перейдите на вкладку Snapshots;
2. Выделите снапшот для клонирования, нажмите кнопку Clone;
3. Откроется окно диалога клонирования ВМ из снапшота, схожее с окном диалога создания новой ВМ;
4. Минимально необходимо указать имя клонированной ВМ в поле Name. Остальные свойства вы можете изменить по необходимости;
5. Нажмите ОК для создания клонированной ВМ.

Можно отслеживать статус создаваемой машины в разделе Virtual Machines. Когда он изменится на Down, можно запускать новую ВМ.

Клон является точной копией ВМ, содержащей все ее данные и специфичные настройки. Если вы не хотите создавать точную копию ВМ, а хотите создать машину с похожей конфигурацией, вы можете использовать снапшот для создания «запечатанного шаблона» («sealed template») с очисткой образа ВМ от уникальных данных, и затем создавать новые ВМ из этого шаблона. Для создания шаблона из снапшота используйте кнопку Make Template вместо Clone. Работа с шаблонами описана в разделе «Шаблоны и клонирование» данного курса.
