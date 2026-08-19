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

Note
{: .label .label-blue }
Pirate has two shielded pools: **Sapling** (`zs1...` addresses) and **Ironwood**, the network upgrade that activated the Orchard shielded protocol (`pirate1...` addresses). Consolidation is configured per-pool; sweep and change-address routing work across both.

### Change Address

#### changeaddress
Routes all `z_sendmany` change - regardless of whether the spend was made from a Sapling or Ironwood source address - to a single designated Sapling or Ironwood address, instead of letting the wallet auto-derive a fresh internal change address for each transaction.

There is no consensus rule tying a change output's pool to the spend's pool, so one address can be used to collect change from both.

Default is unset (auto-derived internal change address per transaction). The wallet must hold the spending key for the address you configure, and it is validated at startup - the daemon will refuse to start if the address is invalid or you don't hold its spending key.

`changeaddress=zs1.....`

Note
{: .label .label-blue }
This is of particular interest to exchanges and payment processors that want all change consolidated onto one address they actively monitor, rather than scattered across auto-generated internal addresses.

### Delete Transactions

#### deletetx
Enables or disables transaction deletion.

Default is 0. Set to 1 to enable.

`deletetx=1`

#### deleteconflicttx
Automatically deletes conflicted (double-spent/replaced) wallet transactions as they're detected, independently of the periodic deletion pass below.

Default is 1 (enabled). Set to 0 to disable.

`deleteconflicttx=0`

#### deleteinterval
Delete transactions every N blocks during initial block download and as new blocks arrive.

Default is 10000. Set accordingly as per your needs. For example a miner would not want to keep old transactions for more than 100 blocks.

`deleteinterval=100`

#### keeptxnum
Keep the last N transactions.

Default is 200. Set accordingly as per your needs. For example an exchange might want to keep the last 1000 transactions.

`keeptxnum=1000`

#### keeptxfornblocks
Keep transactions for at least N blocks, regardless of `keeptxnum`.

Default is 10000. The daemon enforces a floor of 100 blocks (one past the maximum reorg length) - values below that are silently raised to 100.

`keeptxfornblocks=1000`

### Consolidation

Auto note consolidation merges N existing notes (zutxos) for a given address, or all addresses in a pool, into fewer notes. Sapling and Ironwood consolidation are configured and toggled independently.

Note
{: .label .label-red }
The old pool-agnostic `consolidation`, `consolidationtxfee` and `consolidateaddress` parameters have been removed. The daemon will refuse to start if it finds them in PIRATE.conf - use the per-pool parameters below instead.

#### saplingconsolidation / ironwoodconsolidation
Enables auto consolidation for the Sapling pool and/or the Ironwood (Orchard) pool. Enable either, both, or neither independently.

Default is 0 for both. Set to 1 to enable.

`saplingconsolidation=1`
`ironwoodconsolidation=1`

#### saplingconsolidationtxfee / ironwoodconsolidationtxfee
Fee amount in arrrtoshis used to send consolidation transactions for each pool.

Default is 10000 arrrtoshi (or 0.0001 ARRR) for both. A fee of 0 can also be used, but there is a chance not all 0-fee transactions will be included in blocks by miners.

Note
{: .label .label-blue }
Having the standard txfee also helps the anonset and blends your consolidation transactions with the rest of them, rather than sticking out.

`saplingconsolidationtxfee=10000`
`ironwoodconsolidationtxfee=10000`

#### saplingconsolidationinterval / ironwoodconsolidationinterval
Interval in blocks between consolidation runs for each pool.

Default is roughly one week's worth of blocks for both. Must be at least 1 if set.

`saplingconsolidationinterval=1000`
`ironwoodconsolidationinterval=1000`

#### consolidatesaplingaddress / consolidateironwoodaddress
Address for which notes should be consolidated, one option per pool.

Default is none, therefore all notes from every address in that pool will be consolidated to their own address.

Note
{: .label .label-blue }
This setting is of particular interest to exchanges to consolidate notes on the zaddr(s) used for withdraws.

`consolidatesaplingaddress=zs1.....`
`consolidateironwoodaddress=pirate1.....`

#### consolidationtargetqty
Minimum number of notes an address must hold before auto-consolidation will process it. Shared across both pools.

Default is 100. Minimum allowed value is 2.

`consolidationtargetqty=20`

### Sweep
As opposed to **consolidation**, sweep moves all notes from every shielded address into one designated address. Sapling and Ironwood addresses are both supported by a single, unified set of parameters.

#### sweep

Default is 0. Set to 1 to enable. Setting `sweepaddress` also enables sweep implicitly.

`sweep=1`

Note
{: .label .label-blue }
When **sweep** is enabled, consolidation for any pool you've enabled is automatically restricted to run only against the sweep address, regardless of what you set in `consolidatesaplingaddress`/`consolidateironwoodaddress`.

#### sweepaddress
The address to sweep funds to. Accepts either a Sapling or an Ironwood address - exactly one must be specified.

`sweepaddress=zs1.....`

Note
{: .label .label-red }
The old `sweepsaplingaddress` and `sweepironwoodaddress` parameters have been removed. The daemon will refuse to start if it finds them in PIRATE.conf - use `sweepaddress` for either pool instead.

#### sweeptxfee
Fee amount in arrrtoshis used to send sweep transactions.

Default is 10000 arrrtoshi (or 0.0001 ARRR). A fee of 0 can also be used, but there is a chance not all 0-fee transactions will be included in blocks by miners.

`sweeptxfee=10000`

#### sweepinterval
Number of blocks between automatic sweep runs.

Default is roughly 15 minutes' worth of blocks. Must be at least 1 if set.

`sweepinterval=100`
