# Подготовка multipath

На хосте выполните следующие команды:

```
mpathconf --enable
systemctl enable multipathd
systemctl start multipathd
```

Затем перезапустите хост и проверьте, что multipath активен и LUN'ы отображаются:

```
systemctl status multipathd
multipath -ll
```

{% hint style="info" %}
Если диск нужного размера отсутствует:\
1\. Проверьте корректность маппинга на СХД\
2\. Убедитесь, что диск презентован серверу\
3\. Выполните процедуру переобнаружения дисков
{% endhint %}

### Процедура переобнаружения дисков

1. Узнайте количество host bus адаптеров, которые есть на сервере.

Для iSCSI/общее сканирование:

```
ls /sys/class/scsi_host/
Пример вывода: host0 host1
```

Только для Fibre Channel:

```
ls /sys/class/fc_host/
```

2. Запустите сканирование на каждом адаптере.

Замените `host0`, `host1` на фактические имена из предыдущего шага.

Для iSCSI:

```
echo "1" > /sys/class/scsi_host/host0/issue_lip
echo "- - -" > /sys/class/scsi_host/host0/scan
echo "1" > /sys/class/scsi_host/host1/issue_lip
echo "- - -" > /sys/class/scsi_host/host1/scan
```

Для Fibre Channel:

```
echo "1" > /sys/class/fc_host/host0/issue_lip
echo "- - -" > /sys/class/scsi_host/host0/scan
echo "1" > /sys/class/fc_host/host1/issue_lip
echo "- - -" > /sys/class/scsi_host/host1/scan
```

3. Перезапустите службу multipathd:

```
systemctl restart multipathd
```

4. Проверьте, что необходимый диск стал доступен:

```
multipath -ll
```

### Альтернативный метод сканирования

Этот метод полезен, если нужно принудительно обновить информацию об определённом устройстве.

1. Определите WWID проблемного устройства командой:

```
multipath -ll
```

Найдите в выводе WWID нужного LUN (например, `3600a09803831445855244b4a784e6651`). 2. Удалите устаревшие multipath-устройства:

```
multipath -f 3600a09803831445855244b4a784e6651
```

3. Убедитесь, что целевые устройства удалены из системы:

```
multipath -ll
```

4. Выполните сканирование и повторное обнаружение новых путей:

```
rescan-scsi-bus.sh
```

5. Проверьте, что необходимый диск стал доступен:

```
multipath -ll
```
