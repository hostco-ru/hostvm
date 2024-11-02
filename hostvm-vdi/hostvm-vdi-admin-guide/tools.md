---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Инструменты

> Данный функционал доступен начиная с версии брокера 3.6.

Раздел "Инструменты" доступен в меню пользователя (только для учетных записей с ролью _Администратор_):



<figure><img src="../../.gitbook/assets/tools-1.png" alt=""><figcaption></figcaption></figure>

## Журналы <a href="#logs" id="logs"></a>

Здесь отображается перечень доступных для загрузки журналов брокера.

Вы можете скачать их по отдельности, нажав на иконку загрузки рядом с нужным файлом, либо архив всех журналов целиком, нажав на кнопку "Скачать все".

<figure><img src="../../.gitbook/assets/tools-2.png" alt=""><figcaption></figcaption></figure>

## REST API Обозреватель <a href="#rest-api-explorer" id="rest-api-explorer"></a>

Обозреватель позволяет выполнять запросы к REST API брокера прямо из браузера.

> Дополнительную информацию вы можете узнать в [Руководстве по REST API](../rest-api-guide/)

Вы можете создать запрос как полностью вручную, так и используя готовые шаблоны.

Чтобы создать запрос вручную:

* выберите HTTP метод;
* задайте Url;
* при выборе PUT или POST метода запроса укажите параметры тела запроса в появившейся форме;
* нажмите кнопку "Запрос".

После выполнения запроса вы увидите код состояния HTTP-ответа и сам ответ сервера.

<figure><img src="../../.gitbook/assets/tools-3.png" alt=""><figcaption></figcaption></figure>

Шаблоны запросов разделены на тематические блоки. Раскройте нужный блок и выберите запрос из появившегося списка.

При нажатии на знак вопроса появится дополнительная информация с описанием запроса, а также параметров path и тела запроса (при наличии).

<figure><img src="../../.gitbook/assets/tools-4.png" alt=""><figcaption></figcaption></figure>

Чтобы создать запрос из шаблона:

* нажмите на нужный шаблон, данные из него подставятся в форму запроса;
* задайте нужные значения для параметров path и тела запроса (при наличии);
* нажмите кнопку "Запрос".

Для некоторых запросов, помимо кода состояния HTTP-ответа и полного ответ сервера будет доступно поле "Обзор" с перечнем найденных элементов в сокращенном формате (имя и id). Id элемента можно скопировать, нажав на иконку рядом, и использовать для дальнейших запросов.

<figure><img src="../../.gitbook/assets/tools-5.png" alt=""><figcaption></figcaption></figure>

## Управление сервисами <a href="#service-management" id="service-management"></a>

Здесь отображается перечень всех назначенных пользователям сервисов.

> Кнопка "Сбросить" активна только для типов сервисов, поддерживающих сброс.

Вы можете перезапустить виртуальную машину или сбросить сессию пользователя, для этого выберите один или несколько (с помощью Shift или Ctrl) элементов из списка и нажмите кнопку "Сбросить":

<figure><img src="../../.gitbook/assets/tools-6.png" alt=""><figcaption></figcaption></figure>
