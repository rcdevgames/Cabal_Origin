# Cabal Online (Self-Hosted Server) in ALPINE LINUX

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

> Cabal Server with Self-Hosted Zerotier Controller

## Table of Contents

- [Docker](#Docker)
- [Zerotier-Installation](#Zerotier-Installation)
- [Cabal-Server-Installation](#Cabal-Server-Installation)
- [Episode2](#Episode2)
- [Episode8](#Episode8)

## Docker

Install [Docker]
```cmd
   apk add --update docker openrc
```
```cmd
   rc-update add docker boot
```
```cmd
   service docker start
```
```cmd
   service docker status
```
```cmd
   apk add docker-compose
```

## Zerotier-Installation

1. Clone the repository:
   ```cmd
   git clone https://github.com/chaaanito/Cabal_Origin.git
   
2. Installing ZTNET Container:
   **Change POSTGRES_PASSWORD for security
   ```cmd
   cd Cabal_Origin/ztnet
   docker-compose up -d
   
3. Access ZTNET Controller
   ```
   http://localhost:3000/
*Create an Account and Create a Network

4. Installing Local Zerotier Client to join the network you have created.
   ```cmd
   wget https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/zerotier-one-openrc-1.10.2-r0.apk
   wget https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/zerotier-one-1.10.2-r0.apk
   apk add zerotier-one-1.10.2-r0.apk 
   apk add zerotier-one-openrc-1.10.2-r0.apk 
   rc-update add zerotier-one boot
   service zerotier-one start

5. Change Local ZeroTier Client Port
   ```cmd
   nano /var/lib/zerotier-one/local.conf
   ```
   COPY & PASTE in PuTTy
   ```cmd
   {
     "settings": {
       "primaryPort": 9994
     }
   }
   ```
5. Joining ZeroTier Network
   ```
   zerotier-cli join (yourNetworkId)
   ```
## Cabal-Server-Installation
## Episode2:
```cmd
   git clone https://github.com/artem-alekseev/cabal_server_ep2.git
   cd cabal_server_ep2
   ```
### Environment

Copy .env.example to .env and configurate .env

```
DB_PASSWORD=StronG_db_passw0rd // Use a strong password for mssql with special symbols, numbers, and uppercase symbols
DB_PORT=1433 // Database port
PUBLIC_IP=this.your.server.ip // IP server
EXP_RATE=100 // Enter EXP rate multiplier, e.g. 5 for 5x 
SEXP_RATE=100 // Enter Skill EXP rate multiplier
CEXP_RATE=100 // Enter Craft EXP rate multiplier
DROP_RATE=2 // Enter drop rate multiplier (over 5 is bad)
ALZ_RATE=100 // Enter Alz rate multiplier
BALZ_RATE=100 // Enter Alz bomb rate multiplier
PEXP_RATE=100 // Enter Pet EXP multiplier
ITEMS_PER_DROP=2 // Enter number of items per drop
CONFIG_TYPE=1
```

#### CONFIG_TYPE
```
1 - Mercury (1 chan. Premium)
2 - Venus  (1 chan. PK)
3 - Mars (1 chan. War)
4 - Jupiter (1 chan. Tierra Gloriosa)
5 - Saturn (2 chan. Normal/TG)
6 - Neptune (3 chan. Normal/War/Tierra Gloriosa)
7 - Pluto (3 chan. PK/War/Tierra Gloriosa)
8 - Leo (4 chan. Normal/PK/Prem War/Tierra Gloriosa)
9 - Sirius (4 chan. Normal/PK/War/Tierra Gloriosa)
10 - Draco (5 chan. Normal/Prem/Prem PK/Prem War/TG)
11 - Test Server (1 chan. PK)
12 - Duality (2 server, 1 norm and 1 War channel) DONT WORK STABLE
20 - Divinity (3 server, 1 norm and 1 PK channel each) DONT WORK STABLE
```

Otherwise, your container will not start.

## Build containers
```cmd
docker-compose build
```
## Database

Up container
```cmd
docker-compose up -d mssql
```

Restore database
```cmd
docker-compose exec mssql sh restoredb.sh
```
default registered user 
username "admin" password "admin"
## Server

Start server
```cmd
docker-compose up -d server
```
```cmd
docker-compose exec server sh /home/cabal/cabal_config.sh
```
### Stop server
```cmd
docker-compose down server
```
### Service logs

```cmd
server/logs
```

### GM panel/Register Account http://localhost
```cmd
docker-compose up -d --build site nginx
docker-compose exec site composer install --no-cache
docker-compose exec site php artisan migrate --force
docker-compose exec site chmod -R 777 storage
```

### GM panel Set GM
```cmd
docker-compose exec site php artisan set:gm (login|ID)
```
#### example
```cmd
docker-compose exec site php artisan set:gm admin
```
##########################################################################
## Episode8: 
   ```cmd
   git clone https://github.com/artem-alekseev/cabal_server.git
   cd cabal_server
   ```
### Environment

Copy .env.example to .env and configurate .env

```
DB_PASSWORD=password_from_db //Use a strong password for mssql with special symbols, numbers, and uppercase symbols
CONNECT_IP=192.168.1.1 // IP server
EXP_RATE=100 // Enter EXP rate multiplier, e.g. 5 for 5x 
SEXP_RATE=100 // Enter Skill EXP rate multiplier
CEXP_RATE=100 // Enter Craft EXP rate multiplier
DROP_RATE=2 // Enter drop rate multiplier (over 5 is bad)
ALZ_RATE=100 // Enter Alz rate multiplier
BALZ_RATE=100 // Enter Alz bomb rate multiplier
PEXP_RATE=100 // Enter Pet EXP multiplier
WEXP_RATE=100 // Enter War EXP multiplier
ITEMS_PER_DROP=2 // Enter number of items per drop
```

Otherwise, your container will not start.

## Build containers
```cmd
docker-compose build
```
## Database

Up container
```cmd
docker-compose up -d mssql
```

Restore database
```cmd
docker-compose exec mssql sh restoredb.sh
```
## Server

Start database agents
```cmd
docker-compose up -d global_db_agent auth_db_agent cash_db_agent event_db_agent pc_bang_db_agent db_agent_01 rock_and_roll_its
```
Start Global Manager Server
```cmd
docker-compose up -d global_mgr_svr
```
Wait loading Global Manager Server (~20-30 sec)

Start other services

```cmd
docker-compose up -d party_svr_01 chat_node_01 event_mgr_svr login_svr_01 agent_shop_01
```

Start world server (channels)

Premium
```cmd
docker-compose up -d world_svr_01_01
```
War
```cmd
docker-compose up -d world_svr_01_02
```

Start M War channels (M War channels auto restart every 4 hours)
```cmd
docker-compose up -d world_svr_01_03 world_svr_01_04 world_svr_01_05 world_svr_01_06 world_svr_01_07
```

### Stop server
```cmd
docker-compose down
```

### Service status

```cmd
docker-compose ps
```

### Service logs

```cmd
docker-compose logs container_name
```
example
```cmd
docker-compose logs global_mgr_svr
```
