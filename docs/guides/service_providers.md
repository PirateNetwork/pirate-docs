---
layout: default
title: Service Providers
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

# Service Provider Integration
{: .no_toc}

This section provides basic Pirate Chain Integration guidelines for service providers.

## Exchanges

### Install Pirate Daemon
Please see [Installation](../../installation) for Pirate Daemon setup instructions.

### Pirate Daemon Configuration
Please see [Configuration](../../configuration) section, specifically [Advanced Configuration](../../configuration/advanced) and [Examples](../../configuration/examples) sections.

### Key Operations

Best Practices for Exchanges
{: .label .label-blue}
Using 2 different nodes is highly recommended:
- one that will hold the deposit wallet (containing deposit addresses for each user, depending on how your exchange engine works) - deposit\_node
- one that will hold the withdraw wallet (from which withdraws will be issued to users) - withdraw\_node

In order to benefit from this setup, funds from deposit\_node need to be swept regularly (sent) to withdraw\_node.

Following RPC calls will be used most; curl http calls are available and more in-depth documentation can be found in the [RPC / CLI](../../rpc) section.

#### getinfo
This command retrieves useful information in regards to the chain and current daemon state.
Example to retrieve blocks sync-ed by daemon, longestchain received by the network and number of peers connected to:
`pirate-cli getinfo | jq '.|{blocks,longestchain,connection}'`

#### getbestblockhash
Can be used to compare latest block hash with that of other node or [Explorer](https://explorer.pirate.black)

#### [z_getnewaddress](../../rpc/z_getnewaddress)
As of version **5.1.0-rc1** z_getnewaddress generates new diversified zAddresses. Diversified addresses work like an HD wallet.
One rootkey can generate MANY zAddresses.

#### [z_listunspent](../../rpc/z_listunspent)
This gives you a list of unspent notes (shielded utxos or shortly zutxos).
It is recommended to index/cache in a local db the transactions that appear here along with txids and have your system mark them as credited or not - it will make your life easy in the long run if claims for _deposits not arriving_ will arise, but this is left to your own assessment depending on how your engine works

#### [zs_listreceivedbyaddress \<zAddr\>](../../rpc/zs_listreceivedbyaddress)
This gives you a list of all transactions received by an address, however there is no information available as to where it came from - useful for cross referencing txids from this rpc call to txids with indexed txids from **z_listunspent** above.

#### [zs_gettransaction \<txid\>](../../rpc/zs_gettransaction)
This gives you information about a txid.
For funds sent you can see the originating address where funds were sent from (yours) and destination address, as well as amount.
For funds received you only see your address and amount, along with other zk-SNARKs information such as commitment, rk, nullifier, whether a tx is **change** or not.

#### [z_sendmany](../../rpc/z_sendmany)
Example: `z_sendmany "yourWithdrawZsAddress" '[{"address":"userZsAddress", "amount": <amount>}]'`

This will return an operation ID (opid) - it is **recommended** that opids along with related information are indexed/cached in a local database.

opids are cleared on daemon restart or when z_getoperationresult is called
{: .label .label-red}

<br>
IMPORTANT
{: .label .label-red}
You **MUST** take into consideration `change` field when performing transactions - i.e. if 2 node system is not employed and partial withdraws are processed from deposit address of user you must account for change as it goes back to the same deposit address and you mustn't credit it to users' balance in your accounting system hence why using separate deposit/withdraw nodes is recommended to reduce overhead in your exchange engine.

NOTE
{: .label .label-blue}
Upto 200 receivers can be included in ONE z_sendmany operation.
Example: `'[{"address":"zsAddr1","amount":1},{"address":"zAddr2","amount":2}]'`

#### [z_getoperationstatus '\["\<opid\>"\]'](../../rpc/z_getoperationstatus)
This returns a status and other useful information pertaining to a **z_sendmany**
Status can be "success", "failed", "executing", "queued" -> you are interested in "success" and "failed" opids.

#### [z_gettotalbalance](../../rpc/z_gettotalbalance)
Displays total balance of wallet (all addresses).

#### [z_getbalance \<zAddr\>](../../z_getbalance)
Displays balance of single zAddress.

#### [z_getbalances](../../rpc/z_getbalances)
Displays list of zAddresses and their respective balances.

#### [backupwallet](../../rpc/backupwallet)
This method is a hot backup - can be done while daemon is running and operating. It creates a copy of **wallet.dat**.
To be able to use this method you need to have started the daemon with `-exportdir` parameter. Example: `pirated -exportdir=/home/user/piratebackups`

Backup frequency should be in line with your backup and retention policies.

#### [z_exportwallet](../../rpc/z_exportwallet)
Similar to `backupwallet`, **z_exportwallet** needs `-exportdir` set at startup.
This method dumps all addresses and their private keys in a text file.

#### [z_importwallet](../../rpc/z_importwallet)
Imports private keys dumped by _z\_exportwallet_.

#### [z_getnewaddresskey](../../rpc/z_getnewaddresskey)
This method generates a new root key in your wallet. It can be used if you wish to use multiple root keys in one of your nodes.
Having multiple rootkeys in same wallet can decrease performance.

#### [z_setprimaryspendingkey](../../rpc/z_setprimaryspendingkey)
This method tells the daemon which rootkey to use to generate new diversified zAddresses.


### Notes
`confirmations` field from rpc json output when querying a txid will use dPoW(delayed proof of work) confirmations
It is **recommended** to use that over `rawconfirmations`

When using dPoW confirmations, this field stays at **1** until a notarization happens (snapshot on KMD + LTC blockchains, generally every 10-15 mins).
Once a notarization happens **confirmations** == **rawconfirmations** meaning that as long as you have **confirmations > 2** you are safe as service provider from 51% attacks & double spends and will vastly reduce deposit/withdrawals time, making for a great user experience.

Recommended confirmations: anything above 2, but that is left to your own judgement and risk management policies.

