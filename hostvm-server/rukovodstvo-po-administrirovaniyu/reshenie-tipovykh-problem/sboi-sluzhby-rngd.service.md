# Сбой службы rngd.service

1. Проверить установлен ли пакет rng-tools: `rpm -qa rng-tools` , если пакет отсутствует установить командой `yum install rng-tools`&#x20;
2. Перезаписать файл по умолчанию для systemd: `cp /usr/lib/systemd/system/rngd.service /etc/systemd/system`&#x20;
3. Открыть файл `vi /etc/systemd/system`&#x20;
4. Привести строку ExecStart к следующему виду: `ExecStart=/sbin/rngd -f -r /dev/urandom -o /dev/random`&#x20;
5. Перезаписать демон: `systemctl daemon-reload`&#x20;
6. Запустить демон: `systemctl start rngd.service`&#x20;

