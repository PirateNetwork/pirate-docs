---
layout: default
title: z_getnewaddress
parent: RPC / CLI
nav_order: 15
---

## z_getnewaddress

Returns a new diversified shielded address for receiving payments.

```
Result:
"PIRATE_address"    (string) The new diversified shielded address.
```

### Examples:
```
pirate-cli z_getnewaddress 
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_getnewaddress", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
