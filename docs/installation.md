---
layout: default
title: Installation
nav_order: 2
---

## Pirate Installation Guide

### Build from source (recommended)

Please see below the instructions to install Pirate daemon, best practices and tips.

```diff
- !!! IMPORTANT !!!
```
PirateChain (ARRR) is a fully shielded chain and ***ONLY*** uses zs addresses - transparent addresses are ***NOT*** permitted to perform transactions other than generating coinbase (mining) and notarizations by elected KMD notaries; the usual RPC commands generally don't apply

RPC commands for Pirate start with z_ -> see **pirate-cli help** for a list of available commands

***1. Install dependencies***

```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool libncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libdb++-dev ntp ntpdate software-properties-common libevent-dev curl libcurl4-gnutls-dev cmake clang libsodium-dev jq htop -y
```

***2. Clone repository***

```
git clone https://github.com/PirateNetwork/pirate
```

***3. Build daemon***

```
cd ~/pirate 

./zcutil/fetch-params.sh

./zcutil/build.sh -j$(nproc)
```

***4. Set shortcuts for ease of use and start the daemon:***

```
sudo ln -sf /home/$USER/pirate/src/pirate-cli /usr/local/bin/pirate-cli 
sudo ln -sf /home/$USER/pirate/src/pirated /usr/local/bin/pirated

pirated &
```

- on first run of daemon the configuration file is created in ***~/.komodo/PIRATE/PIRATE.conf*** <- you can specify a different location by passing **-datadir=/path/to/data/dir** when running the daemon (***!!!*** if you do so you will need to run that param every time you run/restart the daemon)
