---
description: >-
  Список стандартных портов, используемых различными компонентами и сервисами
  HOSTVM VDI
---

# Список портов

## Внутреннее использование компонентами HOSTVM VDI <a href="#internal-use" id="internal-use"></a>

| Источник            | Назначение                                               | Порт  |
| ------------------- | -------------------------------------------------------- | ----- |
| HOSTVM VDI Брокер   | Сервер БД брокера (MariaDB)                              | 3306  |
| HOSTVM VDI Брокер   | Сервер БД брокера (PostgreSQL)                           | 5432  |
| HOSTVM VDI Брокер   | Агент HOSTVM VDI (виртуальные рабочие столы или серверы) | 43910 |
| HOSTVM VDI Туннелер | HOSTVM VDI Брокер                                        | 443   |
| Агент HOSTVM VDI    | HOSTVM VDI Брокер                                        | 443   |

## Аутентификаторы <a href="#authenticators" id="authenticators"></a>

| Источник          | Назначение             | Порт           |
| ----------------- | ---------------------- | -------------- |
| HOSTVM VDI Брокер | Active Directory, LDAP | 389, 636 (SSL) |
| HOSTVM VDI Брокер | Radius                 | 1812,1813      |
| HOSTVM VDI Брокер | SAML                   | 443            |

## Сервис-провайдеры <a href="#providers" id="providers"></a>

| Источник          | Назначение           | Порт |
| ----------------- | -------------------- | ---- |
| HOSTVM VDI Брокер | HOSTVM               | 443  |
| HOSTVM VDI Брокер | OpenGnsys            | 443  |
| HOSTVM VDI Брокер | OpenNebula/OpenStack | 2633 |
| HOSTVM VDI Брокер | oVirt/RHV/OLVM       | 443  |
| HOSTVM VDI Брокер | Proxmox              | 8006 |
| HOSTVM VDI Брокер | VMware vCenter       | 443  |
| HOSTVM VDI Брокер | XenServer            | 443  |

## Проверка доступности сервисов VDI <a href="#vdi-connectivity" id="vdi-connectivity"></a>

| Источник          | Назначение                     | Порт      |
| ----------------- | ------------------------------ | --------- |
| HOSTVM VDI Брокер | HTML5 RDP, HTML5 RA, HTML5 RDS | 3389      |
| HOSTVM VDI Брокер | HTML5 SSH                      | 22        |
| HOSTVM VDI Брокер | Loudplay                       | 8554      |
| HOSTVM VDI Брокер | PCoIP                          | 443       |
| HOSTVM VDI Брокер | RDP, RemoteApp, RDS            | 3389      |
| HOSTVM VDI Брокер | SPICE                          | 5900-6923 |
| HOSTVM VDI Брокер | X2Go                           | 22        |

## Клиентские устройства и туннели <a href="#clients-and-tunnels" id="clients-and-tunnels"></a>

<table><thead><tr><th width="226.8887939453125">Источник</th><th>Назначение</th><th width="107.2222900390625">Порт</th></tr></thead><tbody><tr><td>Клиентское устройство</td><td>Брокер HOSTVM VDI (портал пользователя)</td><td>443</td></tr><tr><td>Клиентское устройство</td><td>Туннелер HOSTVM VDI (портал пользователя)</td><td>1443</td></tr><tr><td>Клиентское устройство</td><td>Сервисы VDI (виртуальные рабочие столы, приложения, терминальные серверы и т.п.) (прямые подключения)</td><td>*</td></tr><tr><td>Клиентское устройство</td><td>Туннелер HOSTVM VDI (туннельные подключения)</td><td>443</td></tr><tr><td>Клиентское устройство</td><td>Туннелер HOSTVM VDI (HTML5 подключения)</td><td>10443</td></tr><tr><td>HOSTVM VDI Туннелер</td><td>Сервисы VDI (виртуальные рабочие столы, приложения, терминальные серверы и т.п.)(туннельные и HTML5 подключения)</td><td>*</td></tr></tbody></table>

**\*** - в зависимости от используемого протокола подключения
