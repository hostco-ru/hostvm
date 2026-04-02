# Если при установке на хранилище GlusterFS возникли неполадки

1. **Ошибка на этапе выполнения основного скрипта**\
   Если ошибка возникла во время выполнения команды:

```
/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log
```

Выполните очистку узла с помощью Ansible и начните развертывание заново:

```
ansible-playbook /etc/ansible/clean-node.yml
```

2. **Ошибки на этапах выполнения Ansible**\
   Если ошибка возникла на одном из этапов выполнения плейбуков, повторите выполнение только той команды, на которой произошел сбой:

```
ansible-playbook /etc/ansible/make-prepare.yml
```

```
ansible-playbook /etc/ansible/make-gluster-storages.yml
```

```
ansible-playbook /etc/ansible/make-ovirt.yml 
```

3. ​Если на этапе установки engine `/root/script-hosted-engine-deploy | tee -a /root/script-hosted-engine-deploy.log` установка зависает на этапе `Engine VM domain: [rtc.local]rtc.local Enter root password that will be used for the engine appliance: engine`, то подключитесь к консоли сервера не по SSH, а с помощью ipmi(iLO, iDRAC, etc.) и повторно запустите скрипт установки менеджера.

Если устранить проблему не удалось, обратитесь в[ техническую поддержку](https://lk.pvhostvm.ru/) используя [инструкцию](https://lk.pvhostvm.ru/) К обращению приложите лог вывода консоли (если установка проводилась через CLI) и файл `/root/script-hosted-engine-deploy.log`
