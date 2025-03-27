# Мультисессии, окно выбора сессии

#### Следующее поведение со стороны Windows Server является ожидаемым:

#### Возможность открытия нескольких инстансов одного приложения, опубликованного через RemoteApp и HTML5 RemoteApp

Описание: если при удалённое приложение открыто, и пользователь снова закажет это же приложение – откроется ещё один инстанс этого приложения. Старое окно при этом останется активным.

<figure><img src="../../../.gitbook/assets/image (50).png" alt=""><figcaption><p>Публикация через RemoteApp</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (51).png" alt=""><figcaption><p>Публикация через HTML5 RemoteApp</p></figcaption></figure>

#### Несколько приложений в одной HTML5  сессии

Условие: установление значения "**Enabled/Not Configured**" политики "**Restrict Remote Desktop Services users to a single Remote Desktop Services session**".

Описание: если у пользователя есть активная HTML5 сессия, то при заказе ещё одного сервиса предыдущая HTML5 сессия будет закрыта и запущена новая, в которой будут одновременны открыты предыдущее и текущее приложения.

<figure><img src="../../../.gitbook/assets/image (99).png" alt=""><figcaption><p>Два приложения в одной HTML5 сессии</p></figcaption></figure>

#### Окно выбора сессий при запуске нескольких приложений, опубликованных через HTML5 RemoteApp

Условие: установление значения "**Enabled/Not Configured**" политики "**Restrict Remote Desktop Services users to a single Remote Desktop Services session**".

Описание: если у пользователя на стороне терминального сервера есть сессии со статусом Disconnected, то при заказе сервиса пользователю будет предложен выбор сессии. При этом также возможно появление нескольких приложений в одной сессии.

<figure><img src="../../../.gitbook/assets/image (100).png" alt=""><figcaption><p>Окно выбора сессий</p></figcaption></figure>

#### Решение

Запуск приложений как shell сессии.

Решение находится на стадии тестирования, для получения информации о решении необходимо обратиться в техподдержку.

{% hint style="danger" %}
Решение применимо для Windows Server 2019 до сборки 17763.2931 (KB5015018) включительно.
{% endhint %}
