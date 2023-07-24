# Как исправить ошибку: "The 'value' parameter must be a string"?

Данная ошибка возникает при развертывании HOSTVM Manager на FC, если используется СХД с LUN ID, состоящим только из цифр (например, СХД DEPO). \
Чтобы обойти ошибку, нужно воспользоваться установкой HOSTVM Manager через консоль: \
\
`hosted-engine --deploy --ansible-extra-vars=he_offline_deployment=true`&#x20;
