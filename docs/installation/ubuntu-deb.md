---
layout: default
title: Ubuntu/Debian
parent: Installation
nav_order: 1
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Install dependencies

```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool libncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libgtest-dev libqt4-dev libqrencode-dev libdb++-dev ntp ntpdate software-properties-common libevent-dev curl libcurl4-gnutls-dev cmake clang libsodium-dev jq htop -y
```

## Clone repository

```
git clone https://github.com/PirateNetwork/pirate
```

## Build daemon

```
cd ~/pirate 

./zcutil/fetch-params.sh

./zcutil/build.sh -j$(nproc)
```

## Set shortcuts for ease of use and start the daemon

```
sudo ln -sf /home/$USER/pirate/src/pirate-cli /usr/local/bin/pirate-cli 
sudo ln -sf /home/$USER/pirate/src/pirated /usr/local/bin/pirated

pirated &
```


{: .text-red-300} !!! IMPORTANT !!!

On first run of daemon the configuration file is created in ***~/.komodo/PIRATE/PIRATE.conf*** <- you can specify a different location by passing **-datadir=/path/to/data/dir** when running the daemon (***!!!*** if you do so you will need to run that param every time you run/restart the daemon)
