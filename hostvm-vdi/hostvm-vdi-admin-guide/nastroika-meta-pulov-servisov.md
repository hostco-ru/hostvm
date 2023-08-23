# Настройка мета-пулов сервисов

Создание мета-пула позволяет получить доступ к виртуальным рабочим столам или приложениям, созданным из различных пулов сервисов и объединяя их. Эти пулы будут работать вместе, предоставляя различные услуги полностью прозрачно для пользователей.

Для создания мета-пула перейдите в раздел **Пулы -> Мета-пулы**

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

Для настройки мета-пула необходимо будет указать:

Вкладка **Основной**:

<figure><img src="../../.gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

**Имя** (Name): имя мета-пула (это имя будет показано пользователю для доступа к его сервису: виртуальному рабочему столу или приложению)

**Короткое имя** (Short name): если указано, это будет название службы, которая будет показана пользователю. При наведении на него появится содержимое поля «Имя».

**Политика** (Policy):  политика, которая будет применяться при создании пулов сервисов, которые являются частью мета-пула.&#x20;

* **Evenly distributed**:  услуги будут создаваться и потребляться одинаково во всех пулах сервисов, составляющих Мета-пул.&#x20;
* **Priority**:  сервисы с наивысшим приоритетом будут создаваться и потребляться из пула сервисов (приоритет определяется полем «приоритет». Чем меньше значение этого поля, тем больший приоритет будет иметь элемент). Когда сервис пулов достигнет максимального количества сервисов, будут израсходованы ресурсы следующего.&#x20;
* **Greater % available**:  сервисы будут создаваться и использоваться из пула сервисов, имеющего самый высокий процент свободных ресурсов.



Вкладка **Display**:

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

**Привязанный образ** (Associated Image): изображение, связанное с мета-пулом.&#x20;

**Группа пулов** (Pool group): предоставляет возможность группировать различные мета-пулы в группы пулов. Группу необходимо предварительно создать в разделе **Пулы** -> **Группы**.&#x20;

**Видимый** (Visible): если этот параметр отключен, мета-пул не будет отображаться как доступный для пользователей на странице сервисов брокера HOSTVM VDI.&#x20;

**Доступ к календарю запрещен** (Calendar Access denied text): текст, который будет отображаться, когда доступ к мета-пулу запрещен приложением календаря доступа.

**Выбор транспорта** (Transport Selection): в этом свойстве указывается, как транспорты будут доступны мета-пулу:&#x20;

* **Automatic selection**:  доступный транспорт с наименьшим приоритетом, назначенным пулу сервисов, будет доступен в мета-пуле. Выбор транспорта пользователем запрещен.&#x20;
* **Use only common transports**:  существующие транспорты, которые являются общими для всего пула сервисов, будут доступны в мета-пуле.&#x20;
* **Group Transports by label**:  Те транспорты, которые имеют разные и сгруппированные «метки», будут доступны в мета-пуле (это поле есть внутри каждого транспорта на вкладке «Расширенный»)



<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

Вы можете добавить столько пулов сервисов, сколько вам нужно, объединяя сервисы, размещенные на разных платформах виртуализации (VMware, KVM, Azure и т. д.), серверы приложений и статические устройства.



<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

Как и в пуле услуг, здесь вы также будете иметь следующую информацию и разделы:

* **Назначенные сервисы**:  отображает сервисы, назначенные пользователям, позволяя выполнять ручное удаление и переназначение другому пользователю.
* **Группы**:  указывает, какие группы пользователей будут иметь доступ к мета-пулу.
* **Доступ к календарям**:  позволяет применить ранее созданный календарь доступа.
* **Журналы**:  отображаются все проблемы, возникшие в мета-пуле.