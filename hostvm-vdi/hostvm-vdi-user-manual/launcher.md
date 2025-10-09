# Лаунчер HOSTVM VDI

Приложение HOSTVM VDI Лаунчер позволяет получать доступ к витрине сервисов без открытия web-портала в браузере.

После запуска приложения будет представлен следующий интерфейс:

<figure><img src="../../.gitbook/assets/launcher_login_screen.png" alt=""><figcaption></figcaption></figure>

## Смена языка интерфейса <a href="#language" id="language"></a>

Для переключения языка интерфейса (русский/английский) нажмите на иконку в правом-верхнем углу экрана:

<figure><img src="../../.gitbook/assets/launcher_lang_change_btn.png" alt=""><figcaption></figcaption></figure>

## Аутентификация <a href="#authentication" id="authentication"></a>

### Подключение к брокеру <a href="#broker-connection" id="broker-connection"></a>

Для начала работы необходимо пройти аутентификацию. Для этого укажите адрес брокера в соответствующем поле. После ввода адреса брокера станет активна кнопка **Подключиться**, которую необходимо нажать для инициализации подключения:

<figure><img src="../../.gitbook/assets/launcher_broker_conn_btn.png" alt=""><figcaption></figcaption></figure>

В случае неудачного подключения в нижней части приложения отображается информация о возникших проблемах:

<figure><img src="../../.gitbook/assets/launcher_conn_exception.png" alt=""><figcaption></figcaption></figure>

При возникновении ошибок, связанных с сертификатами, вы можете отключить их проверку. Для этого снимите флажок **"Проверять сертификат SSL"** (по умолчанию включен).

**Важно:** После этого все ошибки, связанные с сертификатами, будут игнорироваться, что может представлять угрозу безопасности.

### Учетные данные пользователя и аутентификатор <a href="#credentials-and-authenticator" id="credentials-and-authenticator"></a>

После успешного подключения к брокеру HOSTVM будут отображены новые поля: имя пользователя, пароль пользователя и выпадающий список аутентификаторов:

<figure><img src="../../.gitbook/assets/launcher_auth_creds.png" alt=""><figcaption></figcaption></figure>

Как только будут введены учетные данные пользователя и выбран аутентификатор - кнопка **Вход** станет активна:

<figure><img src="../../.gitbook/assets/launcher_auth_creds_filled.png" alt=""><figcaption></figcaption></figure>

После ее нажатия будет запрошен доступ к витрине сервисов.

## Витрина сервисов <a href="#services" id="services"></a>

В случае успешной авторизации будет отображена витрина сервисов, представляющая из себя пролистываемый список всех сервисов, доступных пользователю:

<figure><img src="../../.gitbook/assets/launcher_services_list.png" alt=""><figcaption></figcaption></figure>

В случае, если у сервиса более одного доступного транспорта, справа от него отображается иконка выпадающего меню, после нажатия на которую отобразятся все доступные дополнительные транспорты:

<figure><img src="../../.gitbook/assets/launcher_transport_menu.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/launcher_trans_menu_expanded.png" alt=""><figcaption></figcaption></figure>

Чтобы выбрать сервис, нажмите на его название. Если для сервиса доступны варианты транспортов, выберите нужный тип из выпадающего списка.

## Страница открытия сервиса <a href="#service-request" id="service-request"></a>

После инициализации будет открыта специальная страница запроса сервиса:

<figure><img src="../../.gitbook/assets/launcher_service_request.png" alt=""><figcaption></figcaption></figure>

После запроса сервиса будет произведен автоматический возврат на витрину сервисов. В случае, если произойдет какая-либо ошибка, она так же будет отображена в нижней части экрана:

<figure><img src="../../.gitbook/assets/launcher_service_exception.png" alt=""><figcaption></figcaption></figure>



