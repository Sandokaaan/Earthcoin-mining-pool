## About 
This project is a fork of BootNOMP https://github.com/SkinnyPeteTheGiraffe/BootNOMP
Patched for daemons core 0.17+

## Instalation & configuration.
A guide for Debian/Ubuntu linux server:
1. Run a full node of coin daemon and let it sync.  Use either a pre-compiled binaries (https://github.com/Sandokaaan/EarthCoin2019/releases/download/v.2.0.4/earthcoin_linux_64bit.zip) or build your own from the source code (https://github.com/Sandokaaan/EarthCoin2019).

2. Install dependency libraries.
```bash
  sudo apt-get update
  sudo apt-get install git redis nano wget curl ntp screen
  sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config libssl-dev
  sudo apt-get install libboost-all-dev npm nodejs libminiupnpc-dev software-properties-common
  wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
  source ~/.bashrc
  nvm install v8.1.4
```

3. Install pool software.
```bash
  git clone https://github.com/Sandokaaan/eacpool.git
  cd eacpool
  nvm use v8.1.4
  npm install
  npm update
  npm audit fix
```

4. Configure yoyr pool settings.
Edit file "config.json". Especially in the section "website" set a port number for the web interface of the pool and domain name or IP address in the line "stratumHost":
```json
      "website": {
        "enabled": true,
        "host": "0.0.0.0",
        "port": 9080,
        "stratumHost": "MY_SERVER_IP_OR_DOMAIN_NAME",
```
Feel free to understand and set any property to custom your pool.<br>
Then edit "pool_configs/earthcoin.json" file and set YOUR_POOL_ADDRESS and YOUR_FEE_ADDRESS lines:
```json
    "address": "YOUR_POOL_ADDRESS",

    "donateaddress": "eRucoCDEkL8duz7Qeg7n6LiV41AL4Xuvt8",

    "rewardRecipients": {
        "YOUR_FEE_ADDRESS": 1.5,
        "eRucoCDEkL8duz7Qeg7n6LiV41AL4Xuvt8": 0.1
    },
```
YOUR_POOL_ADDRESS is used for temporary storage of coins to be paid to miners. YOUR_FEE_ADDRESS is a reward for pool operator (that is for you). Feel free to edit/delete the line "donateaddress" or any line in "rewardRecipients" section.<br>
Then (very important) edit lines 
```json
  "port": YOUR_RPC_PORT_NUMBER,
  "user": "YOUR_RPC_NAME",
  "password": "YOUR_RPC_PASSWORD"
```
Set port number to your daemon rpcport number, username and password. All this values you should find/set in the deamon config file ("./earthcoin/earthcoin.conf"). <B>Attention!</B> Those values are set in two places in the "pool_configs/earthcoin.json" file. Both should be set.</b>
Finaly chose port number for stratum servers:
```json
    "ports": {
        "9001": {
            "varDiff": {
                "minDiff": 65536,
                "maxDiff": 500000,
                "targetTime": 10,
                "retargetTime": 60,
                "variancePercent": 30,
                    "maxJump": 25
            }
        },
        "9002": {
            "diff": 1000000
        }
    },
 ```
In my sample file are set two port - 9001 for variable difficulty 65536-500000 and 9002 for fixed diificulty 1000000. Fell free to understand this settings and set worker difficulty suitable for your miners.

## Usage.
Run your pool by a sequence of commands:
```bash
  screen
  cd ~/eacpool
  nvm use v8.1.4
  node init.js
```
Take a moment to glance at the running logs. If no error message appears, you can disconnect by pressing <code>CTRD + A, D</code>.
Then open in your webbrowser homepage of your pool 
<code>MY_SERVER_IP_OR_DOMAIN_NAME:PORT</code>
(for example http://203.75.31.14:8090/ or http://eac.miner.mydomain.com:8090/)
and get acquainted with the web interface of your pool.

Miners in your pool should use a valid earthcoin address as username and any/empty password. Miner rewards are payd to the address used as the username.
