# Создание и запуск планов восстановления

Перейдите во вкладку **Действия**  и разверните раздел **xrm\_OpenUDS.**&#x20;

Доступны следующие действия с планами восстановления:\


<table><thead><tr><th width="166">Действие</th><th>Описание</th></tr></thead><tbody><tr><td><strong>Generate</strong></td><td><p>Генерация нового плана восстановления. </p><p>При запуске действия необходимо указать уникальное наименование плана (по умолчанию - test). </p></td></tr><tr><td><strong>Delete</strong></td><td><p>Удаление существующего плана восстановления. </p><p>При запуске действия необходимо указать наименование плана (по умолчанию - test). </p></td></tr><tr><td><strong>Fail_Over</strong></td><td><p>Запуск созданного с помощью действия Generate восстановления сервисов на резервной площадке. </p><p>В рамках выполнения действия происходит перенос сервисов после сбоя на резервный брокер (Secondary Broker).</p><p>При запуске действия необходимо указать наименование плана (по умолчанию - test). </p></td></tr></tbody></table>



{% hint style="info" %}
В разделе приведен базовый перечень доступных действий.



Подробное пошаговое описание процесса развертывания доступно в разделе: [Руководство по внедрению в среде HOSTVM VDI](rukovodstvo-po-vnedreniyu-v-srede-hostvm-vdi.md)
{% endhint %}
