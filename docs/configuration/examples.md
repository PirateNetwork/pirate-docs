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

### Example 1 - Consolidation (Sapling & Ironwood) & Default Tx Deletion

The example below will consolidate each Sapling and each Ironwood address's notes (zutxos) to itself, and delete unneeded transactions that are older than 10000 blocks as long as there are more than 1000 transactions in the wallet.

i.e.:
- zs1xyz1 address has 10 notes -> they will be consolidated into 1 note on zs1xyz1
- pirate1xyz1 address has 10 notes -> they will be consolidated into 1 note on pirate1xyz1
- ...
- wallet.dat has 900 transactions out of which 100 unlinked transactions are 20000 blocks old -> 0 transactions will be deleted
- wallet.dat has 1200 transactions out of which 50 unlinked transactions are 10001 blocks old -> 50 transactions will be deleted

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

saplingconsolidation=1
saplingconsolidationtxfee=10000
ironwoodconsolidation=1
ironwoodconsolidationtxfee=10000

deletetx=1
keeptxnum=1000
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```


### Example 2 - One ZAddr Consolidation & Custom Tx Deletion

The example below will consolidate only **zs1xyzC** Sapling address's notes (zutxos) to itself and delete unneeded transactions that are older than 100 blocks as long as there are more than 500 transactions in the wallet.

i.e.:
- zs1xyz1 address has 10 notes -> they will not be consolidated
- zs1xyzC address has 10 notes -> they will be consolidated into 1 note on zs1xyzC
- ...
- zs1xyzN address has 10 notes -> they will not be consolidated
- wallet.dat has 400 transactions out of which 100 unlinked transactions are 200 blocks old -> 0 transactions will be deleted
- wallet.dat has 650 transactions out of which 50 unlinked transactions are 101 blocks old -> 50 transactions will be deleted

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

saplingconsolidation=1
consolidatesaplingaddress=zs1xyzC
saplingconsolidationtxfee=10000

deletetx=1
deleteinterval=100
keeptxnum=500
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```

### Example 3 - Consolidation, Sweep & Custom Tx Deletion

The example below will sweep every other Sapling and Ironwood address's notes to **zs1xyzS**, consolidate zs1xyzS's own notes into itself, and delete unneeded transactions that are older than 100 blocks as long as there are more than 500 transactions in the wallet.

i.e.:
- zs1xyz1 address has 10 notes -> they will be swept to zs1xyzS
- pirate1xyz1 address has 10 notes -> they will be swept to zs1xyzS
- zs1xyzS address has 50 notes -> they will be consolidated into 1 note on zs1xyzS
- wallet.dat has 400 transactions out of which 100 unlinked transactions are 200 blocks old -> 0 transactions will be deleted
- wallet.dat has 650 transactions out of which 50 unlinked transactions are 101 blocks old -> 50 transactions will be deleted

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

saplingconsolidation=1
saplingconsolidationtxfee=10000

deletetx=1
deleteinterval=100
keeptxnum=500

sweep=1
sweepaddress=zs1xyzS
sweeptxfee=10000
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```

Note
{: .label .label-blue }
`sweepaddress` accepts a Sapling or an Ironwood address interchangeably - the daemon routes swept notes from both pools to whichever one you set. While sweep is enabled, consolidation for any pool you've enabled is automatically restricted to the sweep address.

### Example 4 - Change Address

The example below routes all `z_sendmany` change - whether the spend came from a Sapling or an Ironwood source address - to a single designated address, **zs1xyzChange**, instead of a freshly auto-derived internal address per transaction. The wallet must hold the spending key for zs1xyzChange.

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

changeaddress=zs1xyzChange
EOF

chmod 600 ~/.komodo/PIRATE/PIRATE.conf
```

Note
{: .label .label-red }
The daemon validates `changeaddress` at startup and will refuse to start if the address is invalid or the wallet doesn't hold its spending key. Import/generate the spending key for zs1xyzChange before adding this to PIRATE.conf.
