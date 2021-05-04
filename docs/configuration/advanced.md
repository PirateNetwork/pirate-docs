---
layout: default
title: Advanced Configuration
parent: Configuration
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

## Advanced Configuration Settings
{: .no_toc}
This section is of particular interest to exchanges, miners and payment processors.

These parameters were built in order to ease wallet management automatically through the daemon, reduce wallet.dat bloat (main issue with shielded only transactions) and optimize performance.

The parameters below should be added in PIRATE.conf depending on your needs.

### Delete Transactions

#### deletetx
Enables or disables transaction deletion.
Default is 0. Set to 1 to enable.

`deletetx=1`

#### deleteinterval
Delete transactions every N blocks during inital block download and as new blocks arrive
Default is 1000. Set accordingly as per your needs. For example a miner would not want to keep old transactions for more than 100 blocks.

`deleteinterval=100`

#### keeptxnum
Keep the last N transactions.
Default is 200. Set accordingly as per your needs. For example an exchange might want to keep the last 1000 transactions.

`keeptxnum=1000`


### Consolidation

#### consolidation
Enable auto Sapling note (zutxo) consolidation
Default is 0. Set to 1 to enable.

`consolidation=1`

#### consolidationtxfee
Fee amount in arrrtoshis used to send consolidation transactions.
Default is 0. While using 0 fee may seem economical there is a chance that not all 0 txfee transactions will be included in blocks by miners. Recommended is 10000 arrrtoshi or 0.0001 ARRR which is the standard txfee.
Having the standard txfee also helps the anonset and blends your consolidation transactions with the rest of them, rather than sticking out.

`consolidationtxfee=10000`

#### consolidationsaplingaddress
Z Address for which notes (zutxos) should be consolidated.
Default is none, therefore all notes from each zaddr will be consolidated to their respective zaddrs.

Note
{: .label .label-blue }
This setting is of particular interest to exchanges to consolidate notes on their zaddr used for withdraws.

`consolidationsaplingaddress=zs1.....`
