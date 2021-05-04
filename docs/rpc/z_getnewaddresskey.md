---
layout: default
title: z_getnewaddresskey
parent: RPC / CLI
nav_order: 16
---

## z_getnewaddresskey ( type )
This creates a new sapling extended spending key and
returns a new shielded address for receiving payments.

With no arguments, returns a Sapling address.

```
Arguments:
1. "type"         (string, optional, default="sapling") The type of address. One of ["sprout", "sapling"].
```
```
Result:
"PIRATE_address"    (string) The new shielded address.
```

### Examples:
```
pirate-cli z_getnewaddresskey 
```
```
pirate-cli z_getnewaddresskey sapling
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_getnewaddresskey", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
