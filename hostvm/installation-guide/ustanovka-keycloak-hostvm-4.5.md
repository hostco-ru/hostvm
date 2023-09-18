# Установка Keycloak (HOSTVM 4.5)

{% hint style="info" %}
Первый шаг инструкции выполняется только в случае, если установка Keycloak производится на HOSTVM Manager, обновленный с версии 4.4 на 4.5, в остальных установка начинается с шага №2
{% endhint %}

1. В веб-интерфейсе управляющей машины заходим во вкладку Administration -> Providers, находим ovirt-provider-ovn, нажимаем Edit и переименуем его, например, на ovirt-provider-ovn\_old. В процессе установки он будет создан заново с другими параметрами.\

2. Перед установкой нужно перевести HOSTVM Manager в режим обслуживания, выполнив с ноды команду:\
   `hosted-engine --set-maintenance --mode=global`
3.  Далее подключиться к управляющей машине по ssh и запустить установку с выбором ответов:\
    `engine-setup --otopi-environment="OVESETUP_CONFIG/keycloakEnable=bool:True" --offline`\
    \
    Либо запустить установку с ответами по умолчанию:\


    `engine-setup --otopi-environment="OVESETUP_CONFIG/keycloakEnable=bool:True" --offline –-accept-defaults`
4. После успешной установки зайти по адресу:\
   [https://FQDN/ovirt-engine-auth/admin/](https://fqdn/ovirt-engine-auth/admin/)\
   Логин: admin\
   Пароль: указан при установке
5. Нужно создать realm, для этого в выпадающем списке нажать Add realm

<figure><img src="https://lh3.googleusercontent.com/429rtMky9D_utY4oRSRj_DxmWyoI3Fsk1b-5wzbqHXu1JwWtM70XQXXg9-VSH8yrqOlhb7l4FfOZZQTjQuXM2HExc71elB-_2o9dVBqCbx5wNeBSAUduSYUAMuhGJS6k2kg62uaVFyGp3hf4wgnYBwI" alt=""><figcaption></figcaption></figure>

6. Задать имя ovirt-internal для нового realm, дальнейшая настройка будет производиться в нем.

<figure><img src="https://lh6.googleusercontent.com/q1Esh5pNwAwr3e_-Ft0XrLgVhSx2-wBKVBjO6SEdXdI5ldI6L4GH5GewaWP_eddxNljZqx1JjWFTIcuuBxh6vLhk5-yiNYSmiZh3l2NJgovcpt2hBCA-UKwWDcRqw3_c8yvCN6Vma-6fd6INpOqUz1Q" alt=""><figcaption></figcaption></figure>

7.  Заходим в Groups, нажимаем New, задаем имя группы ovirt-administrator\


    <figure><img src="https://lh5.googleusercontent.com/owyh0mvvbJtDF4oHlfk6Ddt56LcEt8QV_7up3UVAvr9TFhJmJL1YQ3tdXhIdDaPCA5ItijEtpY34BeuUyMNeHOqKa6Pl9blCKtGmI4mkKtyTOkFo-qI1n1ukMQhgRQ4I6JgDC6uldSOIEub4yECr0M4" alt=""><figcaption></figcaption></figure>
8.  Заходим в Users, нажимаем Add user

    <figure><img src="https://lh6.googleusercontent.com/VCudmhZU5ZqmpAZ-BesJnNQWdkrOGRa7SGOFlFGiIGYYI2dEFCURZ9ovoD4rEmfkgls-BRDi-62dYO0syDNxUesA4q5mrRur9-qRQqqfpuuAm4unOr8lm0khq4aeZ7nJAIjQe5XcHFM7QSmhL_fRWSw" alt=""><figcaption></figcaption></figure>
9.  Задаем имя пользователя admin@ovirt, в выпадающем списке Groups выбираем ранее созданную группу ovirt-administrator\


    <figure><img src="https://lh6.googleusercontent.com/uOtJ02xXZu1EM-pQGHHVjSYdxaJT7d95z14vr7rSaLr0MYsfG0Lxuj3W29lM0P518V9bsM7zDGItSYQh5eta07j0BCkVpWOQSJlFFD3C_nkhQDuu8vOilODkmVk1w89dEe1fI0i_Q6qTLr-WYinv1cY" alt=""><figcaption></figcaption></figure>
10. Проверяем, что пользователь успешно создан, нажам View all users, после чего нажимаем Edit:\


    <figure><img src="https://lh6.googleusercontent.com/TPrxXdg4gC0oRksVTkbvLtVqs3juR0u-3SZuMfXlckVMQ8oPYHeV2uf08BLgQRvVz-AW6INcRyLT3LiwmLE1r9VJ5EsLElCcy9Yi4rWgXB-md7T8LONa4yZ8fPJ7mCx4yIe6b-lRzbVeYRrgsu7OOho" alt=""><figcaption></figcaption></figure>
11. Во вкладке Credentials задаем пользователю пароль, флаг Temporary говорит о том, что при первом входе будет необходимо изменить пароль\


    <figure><img src="https://lh5.googleusercontent.com/5CILD9ezVGCtnyA68XK1Lln25PmT1u8RHtQRj-eUqsPwXlOLjySeV7Fik50zwV-7g5urgBa-XpWfcCI3_QaESFTARm_l8-qbZ7CzdKOYPY6Iot_vcxnJhgb6vWlAwqZhikr4GgqR2QKi8bw3uaBXYYw" alt=""><figcaption></figcaption></figure>
12. После ввода пароля нажимаем Set password:\


    <figure><img src="https://lh6.googleusercontent.com/lUVXeAYuAjn3lsqRAbnJsp3RnB_mTyYlDvmFzg_JJd8kimjZ6aVJp8JRT-eLP5faWGitg1xDBovgk6O7bcSs0HbmR7YmzARUdcfRlw-zg7OaV8pkA0Gl0QVaqsKKFmFKyQA47soNFP_jS9Bfv0BEXqY" alt=""><figcaption></figcaption></figure>
13. Переходим во вкладку Clients и нажимаем Create\


    <figure><img src="https://lh5.googleusercontent.com/p93ugFQm9irgC0DtK5K02NZ70xy6IaOF4WKsDAlygQ7ard1KjmUs_aPrM6dkyyxySnKwjDzM6zzDQci915znG7d07ocbNi7o_-Lp9tjZKcb3chSk4TKYtGsTsaflVEd1lqlkLbEoQ3-Ubf57yjJjbh8" alt=""><figcaption></figcaption></figure>
14. Задаем Client ID: ovirt-engine-internal, в качестве Root URL указываем https://FQDN управляющей машины\


    <figure><img src="https://lh4.googleusercontent.com/oSy8kNe4118lwhqd2T0Nxf5CBBxPJCpWoiCwaaXgEgMNnLpMrsfHhU1W8MGbwGPqM9wN1QkbaROrLVnRN9ceZ2v_mNYetf56FyP4b1ERgQ1Y2t2EpDvkt5UoJgmPzFuu26obmwI8_EptHXOoztWHtk8" alt=""><figcaption></figcaption></figure>
15. Общие настройки клиента выглядят следующим образом:\


    <figure><img src="https://lh4.googleusercontent.com/Y5QbUujsL3_Oayu2k90nvpRN0Dsx8uR5OVuiVqZVg45nM_5B0RwBmeRBAkgGpxmpI_gP8v4-HJGDRat9mJOJVkz2012W7t_Af3Wyev00Q_-aN_pnpnB9eF6egarz3zoLd-OvmwZszYw09wrdLevBO2Y" alt=""><figcaption></figcaption></figure>
16. Переходим во вкладку Mappers и нажимаем Create:\


    <figure><img src="https://lh5.googleusercontent.com/1FlMXbl6Jd85YP08rEnQw9DpBSLnwxK2Mnhjk8kS3LfRjx7pDE8g73IYDCOo0bjjwI09JyCCHreJ4mh92eM5z0Svf0dTwOGDfZcyFXclRVoloAke9ECAb4fFdwWwuqsT5vHH4hqB7sTt_99ZMDSVcaI" alt=""><figcaption></figcaption></figure>
17. Создаем username со следующими параметрами, нажимаем Save:\


    <figure><img src="https://lh4.googleusercontent.com/wF7qEi020eGBp5C-bV9HN0Zk-CPm3VibyrsJ4Udaf1ED-mXlI9fdhVcHOlj79YCoxtozZnxbHpeYSe259iT1ksJr8AVk9SYp5QabWmCMqbBpUsYbNjet6w7Oi1UB-2uC5-Cwg5fAEpxHOS4a-0EtFxs" alt=""><figcaption></figcaption></figure>
18. Создаем groups со следующими параметрами, нажимаем Save:\


    <figure><img src="https://lh5.googleusercontent.com/iTAC1QYK7RPaokIgx7Shw-qUWZD2a9OWsobP0kBASsXoYjlvr2UR5NAhtb2YfkvrNaFb4vVGau2EAoSOx9UUegVH6326ainyztAYhg4lpxuDrwLxjII-v9_HLN3lrLhrj9UntQ69yBDUKzUTeKe7ylo" alt=""><figcaption></figcaption></figure>
19. Создаем realm role со следующими параметрами, нажимаем Save:\


    <figure><img src="https://lh3.googleusercontent.com/VxfxxP4J9Y6tgtiM8Now37AsYnV-IPtLmjhbXWS_bsFFMwrfmsyg9w7qIQCkx2p6wnvigNzeTpzGNfMXcp4UkOUw-AYAJgeffK9GivoZi9Sd5hiXYRFEn6MZJEIcsFZHu8iFxA7B79rKhUl_8Xa8xQk" alt=""><figcaption></figcaption></figure>
20. После настройки Keycloak перезагружаем сервис на управляющей машине:\
    `systemctl restart ovirt-engine`
21. Выводим HOSTVM Manager из режима обслуживания из консоли гипервизора:\
    `hosted-engine --set-maintenance --mode=none`

Веб портал будет доступен по ссылке:\
[https://FQDN/ovirt-engine\
](https://fqdn/ovirt-engine)Логин: admin@ovirt\
Пароль: указан при настройке Keycloak

\
