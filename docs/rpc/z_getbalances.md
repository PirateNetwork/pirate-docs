---
layout: default
title: z_getbalances
parent: RPC / CLI
nav_order: 13
---

## z_getbalances ( minconf includeWatchonly )

Returns array of wallet sapling addresses and balances.
Results are an array of Objects, each of which has:
{address,balance,unconfirmed,spendable}

```
Arguments:
1. includeWatchonly (bool, optional, default=false) Also include watchonly addresses (see 'z_importviewingkey')
```
```
Result
[                             (array of json object)
  {
    "address" : "address",          (string) the transaction id 
    "balance" : n             (numeric) confirmed address balance
    "unconfirmed" : n             (numeric) unconfirmed address balance
  }
  ,...
]
```

### Examples
```
pirate-cli z_getbalances 
```
```
pirate-cli z_getbalances false
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_getbalances", "params": [true] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
