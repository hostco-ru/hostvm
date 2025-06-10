# Процесс установки

При загрузке откроется меню выбора действия. За 60 секунд выберите Install HOSTVM Node 4.5.4. Если за 60 секунд после загрузки не выбрать данный пункт, то начинается тестирование ресурсов сервера и только после этого начнется установка. Остановить тестирование ресурсов сервера возможно через нажатие клавиши esc.

<figure><img src="https://lh6.googleusercontent.com/LT2vMvJEIviVuAhGyPxkHGA3O4nI4Izy62cZ_Eq5ArSzge9EoKXBPpzKuk-lJU1zsXbPm0kYD4Ji94ruNV97cRStQmRcFOFG2JECqUsX7KBfW7P13lWQ7_jZ0R0DJLZZR25M5TVN9p_empdSRVIEMBQ" alt=""><figcaption></figcaption></figure>

В случае, если загрузка установщика зависнет, то нужно повторно загрузиться с установочного диска и в стартовом меню действий выбрать пункт «Troubleshooting», затем “Install HOSTVM Node 4.5.4 in basic graphics mode” для запуска установки с с использованием псевдографического интерфейса.

В открывшемся окне выберите английский язык (English), который будет использоваться в интерфейсе установщика.

Выбранный язык не влияет на язык внутри самой операционной системы, которая устанавливается без графической оболочки.

Скриншоты инструкции выполнены в интерфейсе с английским языком. Нажмите Continue.

<figure><img src="https://lh6.googleusercontent.com/Myn7T36gmdK3ZFuDXdblaEtnaa_itG0cTm9-RAk2VIFeQ7IElF2Lwv4ed4t6guygC_u3KIBfOePSp4WeU_zj6cfzIQGoQTZ1tqwIEEwazm-4trSL5guJVI2AjmKlxrT4fAujjP1ALvDKkf9zs8Gssk8" alt=""><figcaption></figcaption></figure>

Далее автоматически открывается меню настроек.

<figure><img src="https://lh6.googleusercontent.com/OINyTav9jNgP904JKtynOqqbKGbPqgYXo_HGjqLCbFeR6oxX5UsmZpD_45LQJn4-pKCjFhwGRcrR415HQPPuRPrDTlEROBYh2sp8qW7Hqv1iKkO6i4ykvibGTSY9i_bGjFSrdsks-gbrnsWQoatBWxs" alt=""><figcaption></figcaption></figure>

**ВАЖНО!: пароль root по умолчанию: HostvmNode**

Перейдите в DATE & TIME, укажите ваш часовой пояс, время и дату. Нажмите Done.

<figure><img src="https://lh6.googleusercontent.com/wmcd-5dh3XFPxLpipBFKJqKMVxF-ESYbmEKeiA-mqV-B2ykOD6yOOHTD0lj0YYsz2Nuk04kjYA1zutmnIspCC_59BbGxYpuQgggiJVOP4Eb45YqTXOys_koQhqmKiFo3ByZP7Tx0m13SQUJ2xRUEdvs" alt=""><figcaption></figcaption></figure>

**ВАЖНО!: Для корректной установки необходимо задать корректное имя хоста, отличное от localhost.localdomain, во избежание возможных ошибок во время установки HOSTVM Manager.**

Перейдите в NETWORK & HOST NAME.

<figure><img src="https://lh4.googleusercontent.com/ddY1nzbq6_gb1OmyZIYESRflfmOipDGNRIU9OM2ZAaEn6FJKkoYUhkVtH4AvmVaLUGO5RDUPTqeSEaEymWAMi21aHL2wn6HYqFVn-zDyQXv1sGnTVgx2F-nCGI23eO8tK6_oog8Gj40VVD9d3ploZog" alt=""><figcaption></figcaption></figure>

Выберите интерфейс, нажмите кнопку _Configure..._. В открывшемся окне перейдите на вкладку  General и отметьте пункт "Connect automatically with priority":

<figure><img src="https://lh4.googleusercontent.com/HLbFZxbxOVez4tNzpEn3gNsR-IAxJt28SEt8_Qqg0fKBSIjeW3upusYlWhNM5ZAz8Xsa-1JWZz_tYQS7vPrN3b5JYiLKjkOP6Wkb2jxXdMY_TkEUobzGgbZYKzwFd9Dq7_DLf_8AtHDQZ0sa7HVUzac" alt=""><figcaption></figcaption></figure>

Далее перейдите на вкладку IPv4 Settings, выберите Method: Manual, введите ip, маску, gw, DNS-сервер. Нажмите кнопку Save.

<figure><img src="https://lh4.googleusercontent.com/zcTbOgq1YXt7IOghVYgOYp3LziIaZawb8nDam6kNxP9QwG2uYiKd2gpdEPRs8RcFf2I53UBngBGviDXMn1lmABIS32alSa3S9sEMb1bGc7UKjGia7Lwj7vz6WmNG9_DCQz4ps728R4bnEefQo-BbcR4" alt=""><figcaption></figcaption></figure>

Переведите тригер возле названия подключения в положение On. В поле Host name введите имя сервера, нажмите кнопку Apply. Нажмите кнопку Done.

<figure><img src="https://lh3.googleusercontent.com/ao5xZxso6Y9IfL23HDrFKmjTRBAUB6kW5BhwTPS_SThx1Ki8Q3r9-RtYWzQzxmaveH6vd4iJUopAjLeSsbll-J5wDK3HL9oRIkGpMuyAwgOqyR7AJ3YJEXu9-0t0jyx1JMlLoXcXFebx7XH2D9DeHlo" alt=""><figcaption></figcaption></figure>

Перейдите в Instalation Destination. Выберите диск на который необходимо выполнить установку. Выберите радиокнопку Custom. Нажмите Done.

**Примечание**: Если в списке доступных устройств не отображаются диски - убедитесь, что данные диски не содержат разделов с неподдерживаемыми Centos 8 файловыми системами.

<figure><img src="https://lh6.googleusercontent.com/LxXZkyxIkDLb2s5dPkgoc6MJ0AnCer7jBV7vs2_ckf9IYdm6WWSwmV3x4VmEAHZEq7k8oq6xxFPxK8UARSff4DEdljV9lauPG_R5kO-DHChvO572JbAOAVRwl_wrSiM_fZTEopl45qbyU_IiFwDWR_c" alt=""><figcaption></figcaption></figure>

После выбора места установки автоматически открывается следующее меню.

<figure><img src="https://lh3.googleusercontent.com/dzhiPwrAqKj2crn3Mn8l5DyJYNTxGOOT7jfxJ0rBXV8DZTUFlQ_VEOWwW3w9Wg0nvfhx0HbXsUam-y3Ln4nQ-Eg0cqQaECFPwp_Tofi007cVABU4Ym-pJ9X72DKLAu2oSN1Q60ZzM62uw0zlLJJukRY" alt=""><figcaption></figcaption></figure>

Если диск уже использовался (имел таблицу разделов), то удалите их, как показано ниже.

<figure><img src="https://lh5.googleusercontent.com/D-iqyCgnhu76fWhk7LmkHNBKRvHQ7CnMqrhQYexQSMyZrdWXA7EVoPOV-sSrw4qHYOO4Ak9QlaeOvnAd9czjRYgQMJlG3zYYFroaNed-No8FaJSQn7gWujW1rRXH2tnokRwyPBWZfmQyKPpRdTNCzT8" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh5.googleusercontent.com/2GrhY5mPM49iFgzqKbSMfTneLD2Cs9NxiTCGOHIHpit4O-UcyhvE4KP8bhVnatohejwUeP-t4sI-ZTmPjkjmgmNw4v5vW80TeaDspfVGNepAKmVHjfF5zqM0YxtZdp8RZXx_OQyL8nI10cPxRY9-MW8" alt=""><figcaption></figcaption></figure>

Когда на диске не останется существующих разделов Из выпадающего меню выберите LVM Thin Provisioning. Нажмите на “+” и создайте раздел /var:

<figure><img src="https://lh3.googleusercontent.com/-FdCLtN71R6eWc6OVfUYaUIPfcV1L0Qj_Nybv3QBSlnF6DL5dyhk-jZ5lRWm7XlcOfTwldZujlfZ9cFn68BPzZljfFR0wt0UpH5QTvXP-mrFW8DFJd4qjusQj7_Fry9PAW-6xhMmE1iGCN2wIc0c98k" alt=""><figcaption></figcaption></figure>

Создайте раздел /boot:

<figure><img src="https://lh5.googleusercontent.com/45QxUCERO0zhYzKMlSmX5VTiqmecFo6UdzeKT-R1outvgGRMkzFtQZiYy5m0MvW2z72ks1fUF5eRTyPwKEB3hOTZGbyzA16FXi70MuQvZCSKcSPyVksrrnpP3_-N8p4Eb_fJEBZiYFQXuunRlHs87cI" alt=""><figcaption></figcaption></figure>

Создайте раздел / (корневая файловая система):

<figure><img src="https://lh6.googleusercontent.com/sjKZPF53aQWBatiIRplbSnhS5_D1upS0-Y2VE7FMg11tpXaQ3BPEBLzZrN1G7juTc63e5j1JReQwzjZ5_-k7IF_kBC2yJk49FTB3uNtQjZP-FGOzEl4j2YyOhbvlEwjTfT9ZwW_UtuVqukKFP7C-YWY" alt=""><figcaption></figcaption></figure>

Создайте раздел swap:

<figure><img src="https://lh4.googleusercontent.com/iYuEuCopd2BMjf81bxyyLPNON9QaoSRnkBnTp6c-no5XhHJgHMSmncGVybo_etEVi6C0wfZRknAaqXdiK3FdGd5KNWM6UuUt1c8qsstSlIDuo33r1cqz-6iGnfKcd1qA7adRH5mgS95ZpqnCSaW6hTQ" alt=""><figcaption></figcaption></figure>

Раздел /data (не менее 90GB) создается только в случае, если установка будет производиться на NFS-шару, находящуюся на локальном диске:

<figure><img src="https://lh3.googleusercontent.com/I9s_x5RVisnNR1RgIL663Qt1eY3Lew5IYsVH-VgVVJk9r-EBzWMoPMb1xVmRmKNmBnAUuvnnMIvbf3uB2v1Wu8V2sqRUqmOStYW6glszidFWZ-ynWypr4aKYxBjXowLpjx9yDIibJTv-lA11OMYGGGY" alt=""><figcaption></figcaption></figure>

Разметка для установки на локальные диски

<figure><img src="https://lh4.googleusercontent.com/jjP6jZdZ569Yq2WmFOmu3FeQRLGcuwT_iu1F4m5Nl_eAHp9Din-97aENtNWeEy8wEEePtrPDH-C3ZGHEGLjPV1j3xBGle7GAIH5noiGRfp7eN2v4WAATgmPRrLZhwl9QkCC2jHjYJ7PqkcKnqDYTrqM" alt=""><figcaption></figcaption></figure>

Остальные разделы, необходимые для корректной работы системы, будут созданы автоматически.\
Для разворачивания виртуальной машины HOSTVM Manager необходим выделенный раздел для домена хранения, размер раздела необходимо задать согласно системным требованиям. Если необходимо, измените поле Desired Capacity раздела /, чтобы изменить размер системного раздела. Не менее 16GB необходимо для раздела /var.

**Примечание:** В случае, если установка производится в системе с включенным режимом UEFI, при разметке необходимо задать дополнительную точку монтирования вида /boot/efi размером в 1 GB.

**Примечание:** оставшееся свободное место вы можете распределить по своему усмотрению либо увеличив необходимые вам разделы, либо создав отдельный раздел через Web-интерфейс HOSTVM Node после установки.

Подтвердите действие кнопкой Accept Changes.

<figure><img src="https://lh6.googleusercontent.com/F_UUeW7mk74Q1qt1pCfhP7nwnlKx-s5SQUA64lH3A4uc327hedY_3ucKYcFv189ztXgqBbqlNjbYQ_Il-gl8LidIcoE7k6zr2cnfDoALxT-8ShIgisA4FBHOb41N-xNGXfJ3gnNgsQaSPVOZ4POFtxM" alt=""><figcaption></figcaption></figure>

В стартовом меню нажмите кнопку Begin Installation, чтобы начать установку.

**ВАЖНО!: пароль root по умолчанию: HostvmNode**

<figure><img src="https://lh4.googleusercontent.com/SSuI6bFzTNHjHZZ1ZGc_SlkmFKOLVIxrVzKjiPhcRJGK7iVKhwLTp7G3AFVR5jlI9InCP3zSWC4BPCh-snugwIVhfU_4QxW0Tnk8A8ls0AwILc2kZy8O7HZzVoJdms-OF7_DSIbsPjEGRXMOpy2Ikik" alt=""><figcaption></figcaption></figure>

Ожидайте окончания установки. После завершения подтвердите перезагрузку нажатием на кнопки Reboot.

<figure><img src="https://lh5.googleusercontent.com/9WvYElqoEt_QoJiI48QOgAyr3_GOHKvcPb-noKvd0XnPYtX8mbJctKbcNHtaRN-FhdRTtOx64C-B8QoylO0lB5DSiaUnoMQr-vXeIdRY7By2np7r4q7narpFwYUZ_GSRVLVF_y7gTex04OV1Yk1tP3w" alt=""><figcaption></figcaption></figure>

\
