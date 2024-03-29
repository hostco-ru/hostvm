# Настройка политик балансировки

HOSTVM позволяет настраивать политики балансировки для распределения ВМ между хостами. Эти политики определяют набор критериев, по которым менеджер виртуализации выбирает хост для запуска ВМ. По умолчанию доступны пять политик: evenly\_distributed, cluster\_maintenance, none, power\_saving, и vm\_evenly\_distributed. Каждая политика имеет набор настраиваемых параметров. Для настройки политики балансировки кластера:

1. Перейдите в раздел Compute > Clusters. Выберите кластер и нажмите кнопку Edit;
2. В открывшемся окне параметров кластера перейдите на вкладку Scheduling Policy, содержащую настройки политики балансировки. По умолчанию используется политика none. Параметры по умолчанию не позволяют запускать ВМ на хосте, чья утилизация CPU превышает 80 % на протяжении 2 минут и более;
3. В поле Select Policy задайте политику для кластера;
4. Каждая политика балансировки имеет свой набор настраиваемых параметров. Например, политика vm\_evenly\_distributed имеет следующие параметры:\
   • HighVmCount - максимальное количество ВМ на хост. При превышении заданного значения хост считается перегруженным. Значение по умолчанию: 10;\
   • MigrationThreshold - размер буфера, после достижения которого начинается миграция ВМ с хоста. Значение по умолчанию: 5;\
   • SpmVmGrace - параметр, определяющий на сколько меньше ВМ запускать на хосте с ролью SPM по сравнению с обычным хостом. Значение по умолчанию: 5;
5. После завершения настройки, нажмите OK для применения политики.
