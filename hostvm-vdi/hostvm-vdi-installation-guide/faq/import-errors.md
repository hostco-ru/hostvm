# Ошибки при импорте

Если при попытке подключения консолью появляется сообщение "Connected to graphic server":

<figure><img src="../../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption><p>Ошибка "Connected to graphic server"</p></figcaption></figure>

В качестве решения необходимо проверить значение свойства Chipset/Firmware Type. (Compute -> Virtual Machines -> Выбрать нужную ВМ -> Edit -> System). Если значение отлично от "Q35 Chipset with BIOS", необходимо привести его к этому состоянию:

<figure><img src="../../../.gitbook/assets/example.jpg" alt=""><figcaption><p>Q35 Chipset with BIOS</p></figcaption></figure>
