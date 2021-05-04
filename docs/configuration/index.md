---
layout: default
title: Configuration
nav_order: 3
has_children: true
permalink: docs/configuration
---

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Initial Configuration

On first run, pirated daemon creates a configuration file located in ~/.komodo/PIRATE/PIRATE.conf with the following parameters:

```
rpcuser=random_rpc_user
rpcpassword=random_rpc_password
rpcport=45453
server=1
txindex=1
rpcworkqueue=256
rpcallowip=127.0.0.1
rpcbind=127.0.0.1
```
