---
layout: default
title: z_listaddresses
parent: RPC / CLI
nav_order: 22
---

## z_listaddresses ( includeWatchonly )

Returns the list of Sprout and Sapling shielded addresses belonging to the wallet.

```
Arguments:
1. includeWatchonly (bool, optional, default=false) Also include watchonly addresses (see 'z_importviewingkey')
```
```
Result:
[                     (json array of string)
  "zaddr"           (string) a zaddr belonging to the wallet
  ,...
]
```

### Examples:
```
pirate-cli z_listaddresses 
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_listaddresses", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
