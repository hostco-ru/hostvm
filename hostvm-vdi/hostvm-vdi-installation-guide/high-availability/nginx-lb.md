# Настройка nginx для балансировки подключений

Данная инструкция описывает альтернативную конфигурацию балансировки подключений к пользовательскому порталу с помощью nginx (без использования HAProxy).

В примере приведено содержание `/etc/nginx/nginx.conf` с балансировкой двух порталов пользователя, находящихся на брокерах.

## Пример конфигурации <a href="#config-example" id="config-example"></a>

```
upstream user_portal {
     #В случае, если пользовательский портал настроен на туннелере, нужно указать порт :1443
     server 10.1.99.18:443; #адрес портала №1
     server 10.1.99.19:443; #адрес портала №2
     least_conn; #пример метода балансировки, по умолчанию используется round-robin
}

map $http_x_forwarded_proto $thescheme {
    default $scheme;
    https https;
}

server {
    listen 443 ssl;

    #Сертификаты генерируются центром сертификации
    #Использование самоподписанных сертификатов в продуктовой среде не рекомендуется
	  ssl_certificate /etc/certs/server.pem; #путь до сертификата
    ssl_certificate_key /etc/certs/key.pem; #путь до ключа

    location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $thescheme;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass https://user_portal;
    }
}

```

