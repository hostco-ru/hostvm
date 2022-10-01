# Действия при установке HOSTVM при отсутствии записей в DNS

В случае, если на DNS-сервере отсутствует запись о ноде HOSTVM, то нужно вручную внести изменения в файл /etc/hosts, для этого отредактируйте файл /etc/hosts на ноде:

\
`vi /etc/hosts`

\
и добавьте в конец файла запись вида:

\
`ip FQDN`

\
Пример:\


<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>
