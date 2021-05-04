---
layout: default
title: z_createbuildinstructions
parent: RPC / CLI
nav_order: 8
---

## z_createbuildinstructions 

```
Arguments:
1. "inputs"                (array, required) A json array of json input objects
     [
       {
         "txid":"id",          (string, required) The transaction id
         "index":n,          (numeric, required) shieldedOutputIndex of input transaction
       } 
       ,...
     ]
2. "outputs"               (array, required) A json array of json output objects
     [
       {
         "address":address     (string, required) Pirate zaddr
         "amount":amount       (numeric, required) The numeric amount in ARRR
         "memo": "string"    (string, optional) String memo in UTF8 ro Hexidecimal format
         ,...
       }
     ]
3. fee                  (numeric, optional, default=0.0001
4. expiryheight          (numeric, optional, default=200) Expiry height of transaction (if Overwinter is active)
```
```
Result:
"transaction"            (string) hex string of the transaction
```

### Examples:
`pirate-cli z_createbuildinstructions "[{\"txid\":\"myid\",\"index\":0\"type\":\"sapling\"},...]"`
