## About 
This project is a fork of BootNOMP https://github.com/SkinnyPeteTheGiraffe/BootNOMP
Patched for daemons core 0.17+

## Instalation.
A guide for Debian/Ubuntu linux server:
1. Run a full node of coin daemon and let it sync.  Use either a pre-compiled binaries (https://github.com/Sandokaaan/EarthCoin2019/releases/download/v.2.0.4/earthcoin_linux_64bit.zip) or build your own from the source code (https://github.com/Sandokaaan/EarthCoin2019).

2. Install dependency libraries.
<code>
  sudo apt-get update
  sudo apt-get install git redis nano wget curl ntp build-essential libtool autotools-dev autoconf pkg-config
  sudo apt-get install libssl-dev libboost-all-dev npm nodejs libminiupnpc-dev software-properties-common
  wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
  source ~/.bashrc
  nvm install v8.1.4  
</code><br>

3. Install pool software.
<code>
  git clone https://github.com/Sandokaaan/eacpool.git
  cd eacpool
  nvm use v8.1.4
  npm install
  npm update
  npm audit fix
</code><br>

4. Configure yoyr pool settings.
Edit file "config.json". Especially in the section "website" set a port number for the web interface of the pool and domain name or IP address:
<code>
      "website": {
        "enabled": true,
        "host": "0.0.0.0",
        "port": <b>9080</b>,
        "stratumHost": "MY_SERVER_IP_OR_DOMAIN_NAME",
        "stats": {
            "updateInterval": 15,
            "historicalRetention": 43200,
            "hashrateWindow": 300
        },
</code><br>  
