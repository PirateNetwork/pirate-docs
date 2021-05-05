---
layout: default
title: z_exportviewingkey
parent: RPC / CLI
nav_order: 10
---

## z_exportviewingkey "zaddr"

Reveals the viewing key corresponding to 'zaddr'.
Then the z_importviewingkey can be used with this output

```
Arguments:
1. "zaddr"   (string, required) The zaddr for the viewing key
```
```
Result:
"vkey"                  (string) The viewing key
```

### Examples:
```
pirate-cli z_exportviewingkey "myaddress"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_exportviewingkey", "params": ["myaddress"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```