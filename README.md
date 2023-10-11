# Cabal Online (Self-Hosted Server) in ALPINE LINUX

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

> Cabal Server with Self-Hosted Zerotier Controller

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Contributing](#contributing)
- [License](#license)

## Installation

1. Clone the repository:

   ```
   git clone https://github.com/username/repo.git
2. Installing ZTNET Container:
   **Change POSTGRES_PASSWORD for security
   ```
   cd ztnet
   docker-compose up -d
   
3. Access ZTNET Controller
   ```
   http://localhost:3000/
*Create an Account and Create a Network

4. Installing Local Zerotier Client to join the network you have created.
   ```
   wget https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/zerotier-one-openrc-1.10.2-r0.apk
   wget https://dl-cdn.alpinelinux.org/alpine/v3.17/community/x86_64/zerotier-one-1.10.2-r0.apk
   sudo apk add zerotier-one-1.10.2-r0.apk 
   sudo apk add zerotier-one-openrc-1.10.2-r0.apk 
   sudo rc-update add zerotier-one
   sudo service zerotier-one start

5. Change Local ZeroTier Client Port
   ```
   nano /var/lib/zerotier-one/local.conf

   COPY & PASTE in PuTTy
   {
     "settings": {
       "primaryPort": 9994
     }
   }

5. Joining ZeroTier Network
   ```
   zerotier-cli join (yourNetworkId)

## Usage
## Configuration
