---
layout: default
title: Migration Guide
parent: Service Providers
grand_parent: Guides
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

# Migration Guide
{: .no_toc}
This section is for service providers to provide guidelines on how to best migrate from old daemon to new release (v5.1.0-rc1 and up)

## Best Practice
Let's assume you have 1 node named **node_OLD** processing operations running old daemon with old zAddresses.

It is recommended that you update Pirate Chain Daemon on **node_OLD** to _v5.1.0-rc1_ and keep old wallet.dat. There is a slight performance improvement and it will help, especially if you are encountering deadlocks or regular "freeze" of daemon.

This update will trigger a rescan and therefore it will be a while until it is operational again, depending on how many zAddresses and transactions you have.

In the meantime, setup a secondary node (let's name this **node_NEW**) with _v5.1.0-rc1_ or above and fresh wallet.dat

## Update node_OLD
```
pirate-cli stop # or komodo-cli -ac_name=PIRATE stop

cd ~/pirate # or git clone https://github.com/PirateNetwork/pirate
git checkout pirate_da
git pull
make clean
./zcutil/build.sh -j$(nproc)
```

### Restart node_OLD
This will trigger a rescan. In order to not waste time on this process it is recommended to use parameter `-rescanheight` to minimize the number of blocks to rescan.
You will have to ascertain the minimum blockHeight required to include all transactions processed by your service.

Example:
You had your first Pirate Chain transactions on block 1,200,100
`pirated -rescanheight=1200000 & # added 100 blocks buffer`

## Setup node_NEW
Depending on the OS follow [Installation](../../installation) instructions with a slight difference that we will build from `pirate_da` branch for now.
Example below for Ubuntu 18.04 LTS

### Install dependencies
```
sudo apt-get update && sudo apt-get upgrade -y
```
```
sudo apt-get install build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool libncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libqrencode-dev libdb++-dev ntp ntpdate nano software-properties-common curl libevent-dev libcurl4-gnutls-dev cmake clang libsodium-dev htop jq -y
```

### Clone repository
```
cd ~ && git clone https://github.com/PirateNetwork/pirate
```

### Checkout pirate_da & build
```
cd ~/pirate
git checkout pirate_da

./zcutil/fetch-params.sh

./zcutil/build.sh -j$(nproc)
```

### Set shortcuts for ease of use and start the daemon
```
sudo ln -sf /home/$USER/pirate/src/pirate-cli /usr/local/bin/pirate-cli 
sudo ln -sf /home/$USER/pirate/src/pirated /usr/local/bin/pirated

pirated &
```

### Create PIRATE.conf
Please follow [Configuration](../../configuration) section, specifically [Advanced Configuration](../../configuration/advanced) and [Examples](../../configuration/examples) sections.

NOTE
{: .label .label-blue}
All configuration parameters can be used as arguments when running **pirated**

i.e.: 
```
pirated -consolidation=1 -consolidationtxfee=10000 -deletetx=1 -deleteinterval=100 -keeptxnum=1100 -rescanheight=1200000 &
```

## zAddress Migration
Based on how your service is designed, it is **recommended** to generate new zAddresses for your users, either by forcing them via GUI or by pre-generating them yourself and letting your users know.
The method used is left to your own judgement as you know your systems & users better.

Once this step is done you can start processing NEW deposits. After node_OLD is in sync and operational you can process old deposits and pending withdraws.

## NOTES
New daemon makes use of diversified addresses, meaning there is **ONE** rootkey for **MANY** zAddresses and it essentially works just like an HD wallet.

You can generate and use multiple rootkeys should you wish, however this will add unnecessary overhead and is **NOT** recommended. 
Best performance is achieved with 1 rootkey.

If you do wish to use multiple rootkeys please read [z_getnewaddresskey](.../../rpc/z_getnewaddresskey) and [z_setprimaryspendingkey](../../rpc/z_setprimaryspendingkey) sections from [RPC / CLI](../../rpc) category.

IMPORTANT
{: .label .label-red}

- Once **node_OLD** has finished processing pending deposits and withdraws it is best to keep it running in "watching" mode to account for the odd deposit to old zAddresses for a period left to your own assessment.
- It is best to keep a backup of old wallet.dat and/or old private keys, just in case there is the odd support query in few months time from a user that sent to an old zAddress.
- There is no visible difference between regular zAddresses and diversified zAddresses (both start with _zs1_), however performance is greatly improved with the latter.
- Please also read main [Service Providers](../../service_providers) section for additional information.

As always, please contact our crew should you need any further assistance.
