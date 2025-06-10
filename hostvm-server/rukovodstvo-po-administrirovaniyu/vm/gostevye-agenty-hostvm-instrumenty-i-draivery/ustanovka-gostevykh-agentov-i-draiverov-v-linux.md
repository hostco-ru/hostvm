# Установка гостевых агентов и драйверов в Linux

Гостевые агенты и драйверы предоставляются через репозитории HOSTVM.

Примечание: ВМ CentOS 8 используют службу _qemu-guest-agent_, которая установлена и включена по умолчанию, вместо службы _ovirt-guest-agent_.

Для ручной установки гостевого агента необходимо выполнить процедуру, описанную ниже:

1\.     Подключиться к ВМ;

2\.     Проверить наличие активного репозитория:

●       для CentOS 6:

```
#yum repolist
```

3\.     Установить гостевой агент и зависимости:

●       для CentOS 6 или 7, служба ovirt-guest-agent:

```
# yum install ovirt-guest-agent-common
```

●       для CentOS 8, служба qemu-guest-agent:

```
# yum install qemu-guest-agent
```

4\.     Запустить и включить службу _ovirt-guest-agent_:

●       для CentOS 6:

```
# service ovirt-guest-agent start
# chkconfig ovirt-guest-agent on
```

●       для CentOS 7:

```
# systemctl start ovirt-guest-agent
# systemctl enable ovirt-guest-agent
```

5\.     Запустить и включить службу _qemu-guest-agent_:

●       для CentOS 6:

```
# service qemu-ga start
# chkconfig qemu-ga on
```

●       для CentOS 7 или 8:

```
# systemctl start qemu-guest-agent
# systemctl enable qemu-guest-agent
```

После завершения установки гостевые агенты и драйверы будут передавать информацию об утилизации ресурсов ВМ в HOSTVM Manager. Дополнительные настройки гостевого агента можно указать в файле _/etc/ovirt-guest-agent.conf_
