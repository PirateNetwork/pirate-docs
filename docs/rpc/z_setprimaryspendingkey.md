---
layout: default
title: z_setprimaryspendingkey
parent: RPC / CLI
nav_order: 27
---

## DOCUMENTATION TO BE UPDATED
## z_setprimaryspendingkey

Set the primary spending key used to create diversified payment addresses.

```
Result: Returns True if the spending key was successfully set.
```

### Examples:
```
pirate-cli z_getnewaddress "secret-extended-key-....."
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_getnewaddress", "params": ["secret-extended-key-....."] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
