---
layout: default
title: Lite Daemon
parent: Guides
nav_order: 7
has_children: true
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# Lite Daemon Server
{: .no_toc}
Lite / Light Daemon server is used by lite and mobile Pirate Chain wallets. This guide does not cover basic server security.

## Pre-requisites

### Install Pirate Daemon
Please see [Installation](../../installation) for Pirate Daemon setup instructions.

### Pirate Daemon Configuration
The following PIRATE.conf is needed for lited to interact with ARRR blockchain, cache blocks and broadcast signed transactions.

```
mkdir -p ~/.komodo/PIRATE

cat << EOF > ~/.komodo/PIRATE/PIRATE.conf
rpcuser=user$(openssl rand -hex 32)
rpcpassword=pass$(openssl rand -hex 32)
server=1
rpcbind=127.0.0.1
rpcport=45453
experimentalfeatures=1
txindex=1
insightexplorer=1
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```

### Download blockchain data
Pirate Chain Daemon needs to be in sync with the network before running lited, therefore while you go through the next steps run `pirated` to sync the blockchain or download [Bootstrap](../bootstrap) here.

NOTE
{: .label .label-blue }
Syncing blockchain from scratch is recommended.

### Install golang
Follow golang installation instructions from [https://golang.org/doc/install](https://golang.org/doc/install) or the instructions below.

NOTE
{: .label .label-blue}
At the time of writing this guide current golang version is go1.16.3 so it is best to follow the official documentation for this section as it might change

```
wget https://golang.org/dl/go1.16.3.linux-amd64.tar.gz

# Extract the archive you downloaded into /usr/local, creating a Go tree in /usr/local/go.
# Important: This step will remove a previous installation at /usr/local/go, if any, prior to extracting. Please back up any data before proceeding.
# For example, run the following as root or through sudo:

rm -rf /usr/local/go && tar -C /usr/local -xzf go1.16.3.linux-amd64.tar.gz

# Add /usr/local/go/bin to the PATH environment variable.
# You can do this by adding the following line to your $HOME/.profile or /etc/profile (for a system-wide installation):

export PATH=$PATH:/usr/local/go/bin

# Note: Changes made to a profile file may not apply until the next time you log into your computer.
# To apply the changes immediately, just run the shell commands directly or execute them from the profile using a command such as source $HOME/.profile.
# Verify that you've installed Go by opening a command prompt and typing the following command:

go version

# Confirm that the command prints the installed version of Go.
```

### Set DNS records
Set DNS record pointing to your public IP of your server or load balancer if you are running in Docker or Kubernetes (i.e. lightd.myubercoollitewall.et)

Please follow the instructions of your DNS provider for this section.

### Install nginx & certbot \*
NOTE
{: .label .label-blue}
This section is required only if you want to use nginx as a reverse proxy and want to use LetsEncrypt certificates via certbot.
If you have your own certificates you can skip this section.

```
sudo apt-get install nginx certbot python-certbot-nginx
```

### Generate LetsEncrypt SSL certificate \*
NOTE
{: .label .label-blue}
This section is required only if you want to use nginx as a reverse proxy and want to use LetsEncrypt certificates via certbot.
If you have your own certificates you can skip this section.

```
sudo certbot --nginx
```
Fill in the blanks depending on your chosen hostname (i.e. lightd.myubercoollitewall.et)

## Setup nginx \*
NOTE
{: .label .label-blue}
This section is required only if you want to use nginx as a reverse proxy and want to use LetsEncrypt certificates via certbot.
If you have your own certificates you can skip this section.

Lited uses grpc and http2 therefore nginx must know how to receive and serve these.

### Create nginx configuration for your host
```
cd /etc/nginx/sites-available/ && sudo mv default default.bak && vim default
```
NOTE
{: .label .label-blue}
If you are uncomfortable using vim, any editor will do.

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;

	root /var/www/html;

	# Add index.php to the list if you are using PHP
	index index.html index.htm index.nginx-debian.html;

	server_name _;

	location / {
		try_files $uri $uri/ =404;
	}
}

server {

	# SSL configuration

	location / {
		grpc_pass grpc://127.0.0.1:9068;
	}

    listen 443 ssl http2; # managed by Certbot

    ssl_certificate /etc/letsencrypt/live/lightd.myubercoollitewall.et/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/lightd.myubercoollitewall.et/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}
server {
    if ($host = lightd.myubercoollitewall.et) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

	listen 80 ;
	listen [::]:80 ;
    server_name lightd.myubercoollitewall.et;
    return 404; # managed by Certbot

}
```
NOTE
{: .label .label-blue}
Replace **lightd.myubercoollitewall.et** with your hostname.

## Build lited
```
cd ~ && git clone https://github.com/PirateNetwork/lightwalletd
```
```
cd ~/lightwalletd && go build -o lited cmd/server/main.go

sudo ln -sf /home/$USER/lightwalletd/lited /usr/local/bin/
```

## Run lited

### With NGINX and LetsEncrypt
```
sudo lightwalletd -bind-addr 127.0.0.1:9068 -conf-file ~/.komodo/PIRATE/PIRATE.conf -no-tls
```

To log lited add **-log-file** to run command 
```
sudo lightwalletd -bind-addr 127.0.0.1:9068 -conf-file ~/.komodo/PIRATE/PIRATE.conf -no-tls -log-file /path/to/logfile
```
NOTE
{: .label .label-blue}
Since we are using NGINX to handle TLS authentication we need to pass **-no-tls** to lited. 

### With your own certificate
```
sudo lightwalletd -bind-addr 127.0.0.1:443 -conf-file ~/.komodo/PIRATE/PIRATE.conf -tls-cert cert.pem -tls-key key.pem
```

To log lited add **-log-file** to run command 
```
sudo lightwalletd -bind-addr 127.0.0.1:443 -conf-file ~/.komodo/PIRATE/PIRATE.conf -tls-cert cert.pem -tls-key key.pem -log-file /path/to/logfile
```

You should start seeing the frontend ingest and cache the Pirate Chain blocks after ~15 seconds.
