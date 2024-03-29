# Редактирование и удаление

Можно редактировать параметры домена хранения через портал администрирования. В зависимости от статуса (active или inactive), для редактирования будут доступны разные наборы полей. Поля Data Center, Domain Function, Storage Type и Format не могут быть изменены.

Когда домен хранения находится в статусе Active, можно редактировать следующие поля: Name, Description, Comment, Warning Low Space Indicator (%), Critical Space Action Blocker (GB), Wipe After Delete, Discard After Delete. Поле Name может быть отредактировано только у активного домена. Все остальные поля могут быть отредактированы также для доменов в статусе inactive.

Когда домен хранения находится в статусе Inactive, можно редактировать все поля кроме: Name, Data Center, Domain Function, Storage Type, Format.

Для удаления домена хранения:

1. Перейдите в раздел Storage->Domains;
2. Нажмите на ссылку в имени домена, чтобы открыть его свойства;
3. Перейдите на вкладку Data Center;
4. Нажмите кнопку Maintenance, затем OK;
5. Нажмите кнопку Detach, затем OK;
6. Нажмите кнопку Remove;
7. Для удаления всех данных, хранящихся на домене, включите опцию Format Domain, i.e. Storage Content will be lost!;
8. Нажмите OK.

Домен хранения будет удален из системы виртуализации.
