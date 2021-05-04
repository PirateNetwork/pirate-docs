---
layout: default
title: Configuration
nav_order: 3
has_children: true
permalink: docs/configuration
---

## Configuration Settings
{: .no_toc }
This section walks you through various configuration settings of Pirate Chain Daemon ranging from regular usage to heavy usage (i.e. mining or exchanges)

### Initial Configuration
{: .no_toc }

On first run, pirated daemon creates a configuration file located in ***~/.komodo/PIRATE/PIRATE.conf*** with the following parameters:

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

This initial configuration is regularly enough for regular users with no heavy usage - lots of zutxos (notes) incoming or outgoing.

Note: you can specify a different location by passing **-datadir=/path/to/data/dir** when starting the daemon (***!!!*** if you do so you will need to run that parameter every time you run/restart the daemon)

Note2: if you are integrating Pirate Chain in an exchange, payment processor or run a mining operation please see Enhanced Configuration & Examples sections.
