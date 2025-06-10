# Подготовка putty к работе

Убедитесь, что требования, описанные на странице Системные требования выполняются.

**В случае установки HOSTVM Manager через CLI,** с помощью программы[ PuTTY](https://www.putty.org), которая доступна в[ наборе дистрибутивов для развертывания решения](https://lk.pvhostvm.ru/Download), под пользователем root подключитесь к серверу.

Перед началом работы рекомендуется астроить логирование сессии putty. Для этого нужно выполнить следующие действия

1. Сохраните имя сервера:

<figure><img src="https://lh5.googleusercontent.com/uiLMT5NuDMe7vhe1JBst781cB5GlQjP9IfEIH_bIBVm4Hj2VWsewVW14-TaX72BBQS6EuSGL5gPBKr_OPc4QiN_08af8CXoVVr759IEZAQZCESFS-klgAj1YYArcimzmMHTHj7vJEWMK4peQ7OL8JLY" alt=""><figcaption></figcaption></figure>

2. Перейдите на вкладку Журнал, выберите `Весь вывод`, укажите путь до файла логов в следующем виде: `C:\path\to\log\hostname-&H-&Y&M&D-&T.log.` Часть `&H-&Y&M&D-&T` указывает, что файл с логом будет создаваться для каждой сессии и автоматически указывать время и дату ее начала:

<figure><img src="https://lh3.googleusercontent.com/j8sDbNO52A31oOloNXyBKdp5SwU1cbrlOcXkBfeQ11J9sgaGWA2-a0DdC8BV6DYgaJH1lxtrDr0fSx1TxmuzEX_PTHeRo-sT3_aznPIzrMbhKuHtqbshrnSVrmu51glCbJGvYLxhXCIgr_jmBw-yz48" alt=""><figcaption></figcaption></figure>

3. Перейдите на вкладку Сеанс, нажмите кнопку `Сохранить`, нажмите клавишу `Enter` чтобы запустить сессию:

<figure><img src="https://lh6.googleusercontent.com/Ct_fVVhuXgqCuSBN9nZLV1sQNorDKQkNZSeNSutnqPDJUoQqC8xKQMOFqzlTZZuoLFVKB9UO4V3-nIaJFuGitnG1f3vgMSA1r-4gbjmKlIfIAu0Q9D5-3sGRTCq1GvWQXEqVg4PqFlKIlYTZO4vCrGE" alt=""><figcaption></figcaption></figure>
