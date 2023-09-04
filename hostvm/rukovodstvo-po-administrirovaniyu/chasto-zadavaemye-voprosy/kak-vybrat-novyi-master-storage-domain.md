# Как выбрать новый Master Storage Domain?

В версиях HOSTVM 4.4.x:&#x20;

1. Выберите нужный домен, нажмите на кнопку с 3 вертикальными точками в правом верхнем углу и выберите "Select as master storage domain"

<figure><img src="../../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

В версиях HOSTVM 4.3.x:

1. Погасите все ВМ, кроме HostedEngine.
2. Отправьте мастер-домен в Maintenance mode: Data Centre -> Storage -> ПКМ -> Maintenance. Новый мастер-домен будет выбран автоматически из свободных.&#x20;
3. Если необходимо выбрать какой-то конкретный домен, отправьте все домены, кроме мастер-домена и нового мастер-домена в Maintenance Mode, после чего отправьте мастер-домен в Maintenance Mode. Оставшийся работающий домен автоматически станет мастер-доменом.
