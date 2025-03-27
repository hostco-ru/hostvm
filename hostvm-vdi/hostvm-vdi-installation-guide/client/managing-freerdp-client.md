---
description: >-
  В статье описаны варианты использования wrapper-скрипта для перехвата и
  добавления параметров клиента xfreerdp.
---

# Управление параметрами клиента FreeRDP



{% hint style="info" %}
Информация ниже основана на логике работы клиента:\
Если по пути _**/usr/bin/udsrdp**_ находится исполняемый файл, то этот путь считается более приоритетным по сравнению со стандартным _**/usr/bin/xfreerdp**_. Таким образом, при запросе сервиса параметры от брокера будут переданы именно _**udsrdp**_.
{% endhint %}

### Передача дополнительных параметров для получения пароля учетной записи пользователя

В случае, когда клиенту необходимо подключиться к опубликованному приложению RemoteApp, но в параметрах запуска **xfreerdp** от брокера не поступает пароль для подключения пользователя - есть возможность получения пароля от пользователя в всплывающем диалоговом окне.

{% hint style="info" %}
Для реализации необходимо наличие установленной утилиты zenity в окружении клиента.
{% endhint %}



Для этого необходимо разместить файл по пути _**/usr/bin/udsrdp**_ со следующим содержимы&#x43C;_**:**_

<pre><code><strong>#!/bin/bash
</strong>catchedArgs=""
for arg in "$@"
do
    catchedArgs="$catchedArgs $arg"
done

/usr/bin/xfreerdp ${catchedArgs} /p:$(zenity --entry --title="Password input" --text="Введите пароль:" --hide-text)
</code></pre>

<figure><img src="../../../.gitbook/assets/image (34) (2).png" alt=""><figcaption></figcaption></figure>

### Исключение параметров для работы с xfreerdp3

По умолчанию брокер передает параметры в формате, адаптированном для xfreerdp версии 2. В 3й версии клиента xfreerdp был изменен формат передачи некоторых параметров, что может приводить к ошибкам при запуске сервиса. Рассмотрим перехват параметров старого формата, передача которых вызывает ошибки запуска.\
\
Для этого необходимо разместить файл по пути _**/usr/bin/udsrdp**_ со следующим содержимым:

```
#!/bin/bash
catchedArgs=""
for arg in "$@"
do
    catchedArgs="$catchedArgs $arg"
done

if [[ $catchedArgs == *"/app:"* ]]; then
    catchedArgs="${catchedArgs///app://app:program:}"
fi

if [[ $catchedArgs == *"/cert-ignore"* ]]; then
    catchedArgs="${catchedArgs///cert-ignore//cert:ignore}"
fi

exclude_catchedArgs=${catchedArgs//\/sec:rdp}
exclude_catchedArgs=${exclude_catchedArgs//\/bpp:24}

/usr/bin/xfreerdp ${exclude_catchedArgs}


```

### Передача и исключение параметров для использования единого транспорта для версий xfreerdp2 и xfreerdp3

В случае, если версии клиента xfreerdp у пользователей могут отличаться, можно избежать необходимости создания дополнительных транспортов подключения, модифицируя передаваемые параметры на основании версии клиента.\
\
Для этого необходимо разместить файл по пути _**/usr/bin/udsrdp**_ со следующим содержимым:

```
#!/bin/bash
catchedArgs=""
for arg in "$@"
do
    catchedArgs="$catchedArgs $arg"
done

version=$(/usr/bin/xfreerdp /version)

major_rdp_version=$(echo "$version" | awk -F'[ .]' '/This is FreeRDP version/ {print $5}')

if [[ -z "$major_rdp_version" ]]; then
    echo "Не удалось установить версию клиента xfreerdp."
    /usr/bin/xfreerdp ${catchedArgs}
else
    echo "Обнаружен клиент xfreerdp$major_rdp_version."

    case $major_rdp_version in
        2)
            /usr/bin/xfreerdp ${catchedArgs}
            echo "$catchedArgs" > /home/anakim/udsrdp.log
            ;;
        3)
            if [[ $catchedArgs == *"/app:"* ]]; then
                catchedArgs="${catchedArgs///app://app:program:}"
            fi

            if [[ $catchedArgs == *"/cert-ignore"* ]]; then
                catchedArgs="${catchedArgs///cert-ignore//cert:ignore}"
            fi

            exclude_catchedArgs=${catchedArgs//\/sec:rdp}
            exclude_catchedArgs=${exclude_catchedArgs//\/bpp:24}

            #exclude_catchedArgs=${exclude_catchedArgs//\/cert-ignore}
            /usr/bin/xfreerdp ${exclude_catchedArgs}
          echo "$exclude_catchedArgs" > /home/anakim/udsrdp.log
            ;;
        *)
            echo "Обнаруженная версия xfreerdp $major_rdp_version не поддерживается."
            /usr/bin/xfreerdp ${catchedArgs}
            ;;
    esac
fi
```
