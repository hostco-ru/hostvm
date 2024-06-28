# Точка входа API

`http(s)://{hostname|IPv4}/uds/rest/`

где `{hostname|IPv4}` - IPv4 адрес или FQDN брокера HOSTVM VDI.

Далее по тексту запросы к API указываются в формате:

**`HTTP метод /api/endpoint`**

Например:

**`POST /auth/login`**

Это означает, что для запроса необходимо использовать метод HTTP `POST`, URI запроса в таком случае будет выглядеть как:

`http(s)://{hostname|IPv4}/uds/rest/auth/login`
