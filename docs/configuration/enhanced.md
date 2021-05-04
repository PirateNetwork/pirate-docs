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

# Advanced Configuration Settings
{: .no_toc}
This section is of particular interest to exchanges, miners and payment processors.

These parameters were built in order to ease wallet management automatically through the daemon, reduce wallet.dat bloat (main issue with shielded only transactions) and optimize performance.

The parameters below should be added in PIRATE.conf depending on your needs.

## Delete Transactions

### deletetx
Enables or disables transaction deletion.
Default is 0. Set to 1 to enable.

`deletetx=1`

### deleteinterval
Delete transactions every N blocks during inital block download and as new blocks arrive
Default is 1000. Set accordingly as per your needs. For example a miner would not want to keep old transactions for more than 100 blocks.

`deleteinterval=100`

### keeptxnum
Keep the last N transactions.
Default is 200. Set accordingly as per your needs. For example an exchange might want to keep the last 1000 transactions.

`keeptxnum=1000`
