---
description: >-
  Данное руководство описывает процесс обновления SSL-сертификатов для сервисов
  OVN, используемых в качестве сетевого провайдера в системе HOSTVM.
---

# Обновления SSL-сертификатов для сервисов OVN на Hosted HOSTVM Manager

Процедура выполняется на управляющей машине Hosted Manager для сертификатов `ovirt-provider-ovn`, `ovn-ndb` и `ovn-sdb` .

Перед генерацией новых сертификатов необходимо определить subject текущих, чтобы новые сертификаты были выпущены с такими же данными

```
openssl x509 -in /etc/pki/ovirt-engine/certs/ovirt-provider-ovn.cer -noout -subject
```

Пример вывода:

```
subject=C = US, O = hostvm.local, CN = engine.hostvm.local
```

Запомните или запишите полученное значение. Оно понадобится на следующем шаге в преобразованном формате.

Сгенерируйте новые сертификаты используя полученное значение subject, преобразовав его формат из `type = value` в `/type=value`. Пароль `--password=mypass` должен оставаться неизменным.

```
/usr/share/ovirt-engine/bin/pki-enroll-pkcs12.sh --name="ovirt-provider-ovn" --password=mypass --subject="/C=US/O=hostvm.local/CN=engine.hostvm.local" --keep-key
/usr/share/ovirt-engine/bin/pki-enroll-pkcs12.sh --name="ovn-ndb" --password=mypass --subject="/C=US/O=hostvm.local/CN=engine.hostvm.local" --keep-key
/usr/share/ovirt-engine/bin/pki-enroll-pkcs12.sh --name="ovn-sdb" --password=mypass --subject="/C=US/O=hostvm.local/CN=engine.hostvm.local" --keep-key
```

Важные примечания:

* Параметр `--password=mypass` должен быть именно таким, как указано.
* Поле `--subject` должно быть в формате: `/type0=value0/type1=value1/type2=...`
* Флаг `--keep-key` гарантирует, что при перевыпуске сертификата будет использован существующий приватный ключ.

После успешной генерации новых сертификатов необходимо перезапустить связанные сервисы, чтобы они начали использовать обновленные сертификаты.

```
systemctl restart ovirt-provider-ovn.service
systemctl restart ovn-northd.service
```

После выполнения этих действий обновление сертификатов OVS будет завершено.
