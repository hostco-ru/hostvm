# Увеличение NFS хранилища

Чтобы увеличить объем хранилища NFS, можно или создать новый домен хранения и добавить его в существующий дата-центр, или увеличить доступное свободное пространство на сервере NFS. Для первого варианта см. п. «Добавление NFS хранилища». Следующая процедура объясняет, как увеличить доступное свободное пространство на существующем сервере NFS:

1. Щелкните Storage -> Domains;
2. Щелкните на имя NFS хранилища. Откроется окно с подробным видом;
3. Щелкните вкладку Data Center и затем щелкните Maintenance чтобы перевести домен хранения в режим обслуживания. Это размонтирует существующий общий ресурс и позволит изменить размер домена хранения;
4. На сервере NFS хранилища измените размер хранилища;
5. В представлении сведений (details view) щелкните по вкладке Data Center и нажмите Activate, чтобы смонтировать домен хранения.

