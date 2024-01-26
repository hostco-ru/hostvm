# Установка Standalone HOSTVM Manager 4.4

В режиме Standalone установка HOSTVM Manager производится на отдельно выделенную физическую машину, в то время как в режиме Self-hosted на физической машине (HOSTVM Node) создается виртуальная машина.

#### &#x20;Предварительные шаги

1. Загрузить скрипт engine-setup44.sh из директории HOSTVM/Misc [Личного Кабинета](https://lk.pvhostvm.ru/) и скопировать его на ноду (например, с помощью Winscp)
2. Выдать права на исполнение:

<pre><code><strong>chmod +x /root/engine-setup44.sh
</strong></code></pre>

3. Открыть файл:

```
vi /etc/yum.repos.d/repo.pvhostvm.ru.repo
```

и добавить строку `module_hotfixes=1` для указанных на скриншоте репозиториев:

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

#### Установка Standalone HOSTVM Manager

Запускаем скрипт:

\
`sh /root/engine-setup44.sh`

\
Дождитесь установки необходимых пакетов.

Далее будут предложены варианты установки:

1. Default – установка с параметрами по умолчанию.
2. Custom – установка с собственными параметрами.

Параметры по умолчанию:

<figure><img src="../../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

После выбора параметров нужно будет ввести пароль для доступа к HOSTVM Manager.

Веб-интерфейс будет доступен:

\
`http://FQDN:80/ovirt-engine`

`https://FQDN:443/ovirt-engine`
