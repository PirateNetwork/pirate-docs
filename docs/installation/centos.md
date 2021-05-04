---
layout: default
title: CentOS
parent: Installation
nav_order: 2
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Enable EPEL repo
We need this for Dev Tools

```
sudo yum install epel-release -y
```

## Install dependencies

```
sudo yum update -y
```
```
sudo yum install git pkgonfig automake autoconf ncurses-devel python wget gtest-devel gcc gcc-c++ libtool patch lbzip2 gmp-devel mpfr-devel libmpc-devel m4 curl make cmake clang unzip boost-devel openssl-devel  libevent-devel jq htop -y
```

### Install gcc 8.3.0
This is a requirement for C11++ libs

```
cd ~
wget https://ftp.gnu.org/gnu/gcc/gcc-8.3.0/gcc-8.3.0.tar.gz
tar -xvf gcc-8.3.0.tar.gz
mkdir ~/gcc-8.3.0-build
cd ~/gcc-8.3.0-build
../gcc-8.3.0/configure --enable-languages=c,c++ --disable-multilib
make -j$(nproc) && sudo make install
```

### Add compiler to path permanently in /etc/profile.
```
sudo cat <<'EOF' >> /etc/profile
export PATH=/usr/local/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH
EOF
```

## Clone repository

```
cd ~ && git clone https://github.com/PirateNetwork/pirate
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


!!! IMPORTANT !!!
{: .label .label-red }

On first run of daemon the configuration file is created in ***~/.komodo/PIRATE/PIRATE.conf*** <- you can specify a different location by passing **-datadir=/path/to/data/dir** when running the daemon (***!!!*** if you do so you will need to run that param every time you run/restart the daemon)
