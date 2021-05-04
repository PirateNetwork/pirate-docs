---
layout: default
title: z_exportkey
parent: RPC / CLI
nav_order: 9
---

## z_exportkey "zaddr"

Reveals the zkey corresponding to 'zaddr'.
Then the z_importkey can be used with this output

```
Arguments:
1. "zaddr"   (string, required) The zaddr for the private key
```
```
Result:
"key"                  (string) The private key
```

### Examples:
```
pirate-cli z_exportkey "myaddress"
```
```
pirate-cli z_importkey "mykey"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_exportkey", "params": ["myaddress"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```

