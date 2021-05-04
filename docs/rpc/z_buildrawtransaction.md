---
layout: default
title: z_buildrawtransaction
parent: RPC / CLI
nav_order: 7
---

## z_buildrawtransaction "hexstring"

Return a JSON object representing the serialized, hex-encoded transaction.
```
Arguments:
1. "hex"      (string, required) The transaction hex string
```

### Examples:
```
pirate-cli z_buildrawtransaction "hexstring"
```

```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_buildrawtransaction", "params": ["hexstring"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
