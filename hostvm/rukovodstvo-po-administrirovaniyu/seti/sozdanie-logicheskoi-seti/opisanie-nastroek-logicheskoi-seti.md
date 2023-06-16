# Описание настроек логической сети

Раздел General:

<table><thead><tr><th width="154">Поле</th><th>Описание</th></tr></thead><tbody><tr><td>Name</td><td>Имя логической сети, которое должно быть уникальным в пределах системы, может состоять из букв в обоих регистрах, цифр, дефисов и подчеркиваний</td></tr><tr><td>Description</td><td>Описание сети. Текстовое поле с лимитов в 40 символов</td></tr><tr><td>Comment</td><td>Комментарий</td></tr><tr><td>Enable VLAN tagging</td><td>Включить поддержку тегированных VLAN. Если активировано, в текстовом поле укажите номер VLAN. Использование опции позволяет назначать несколько логических сетей с разными VLAN на один сетевой интерфейс</td></tr><tr><td>VM Network</td><td>Активируйте опцию, если сеть будет использоваться ВМ. Если сеть предназначена для другого типа трафика (например для сетевого хранилища), оставьте ее выключенной</td></tr><tr><td>MTU</td><td>Значение MTU для логической сети</td></tr></tbody></table>

Раздел Cluster:

<table><thead><tr><th width="137.5">Поле</th><th>Описание</th></tr></thead><tbody><tr><td>Attach/Detach Network to/from Cluster(s)</td><td>Позволяет подключать или отключать сеть для кластеров и указывать, будет ли наличие этой сети являться необходимым для кластера</td></tr><tr><td>Name</td><td>Имя кластера, к которому применяются настройки</td></tr><tr><td>Attach</td><td>Позволяет подключать или отключать сеть для кластера</td></tr><tr><td>Required</td><td>Позволяет указывать, будет ли наличие этой сети являться необходимым для кластера</td></tr></tbody></table>

Раздел vNIC Profiles:

<table><thead><tr><th width="132.5">Поле</th><th>Описание</th></tr></thead><tbody><tr><td>vNIC Profiles</td><td>Позволяет назначать один или несколько профилей vNIC для логической сети. Можно добавлять их с помощью кнопок плюс или минус, имя профиля указывается в первом поле</td></tr><tr><td>Public</td><td>Доступность профиля для всех пользователей</td></tr><tr><td>QoS</td><td>Настройка QoS для профиля vNIC</td></tr></tbody></table>