---
layout: default
title: Configuration Examples
parent: Configuration
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

## Configuration examples
{: .no_toc}

### Example 1 - Consolidation & Tx Deletion

The example below will consolidate each sapling address's notes (zutxos) to itself and delete unneeded transactions that are older than 1000 blocks as long as there are more than 1000 transactions in the wallet.

i.e.:
- zs1xyz1 address has 10 notes -> they will be consolidated into 1 note on zs1xyz1
- zs1xyz2 address has 10 notes -> they will be consolidated into 1 note on zs1xyz2
- ...
- zs1xyzN address has 10 notes -> they will be consolidated into 1 note on zs1xyzN
- wallet.dat has 900 transactions out of which 100 unlinked transactions are 2000 blocks old -> 0 transactions will be deleted
- wallet.dat has 1200 transactions out of which 50 unlinked transactions are 1001 blocks old -> 50 transactions will be deleted

```
mkdir -p ~/.komodo/PIRATE

cat << EOF > ~/.komodo/PIRATE/PIRATE.conf
rpcuser=user$(openssl rand -hex 32)
rpcpassword=pass$(openssl rand -hex 32)
rpcport=45453
server=1
txindex=1
daemon=1
rpcworkqueue=256
rpcallowip=127.0.0.1
rpcbind=127.0.0.1
consolidation=1
deletetx=1
keeptxnum=1000
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```
