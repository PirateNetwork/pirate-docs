---
layout: default
title: z_exportwallet
parent: RPC / CLI
nav_order: 11
---

## z_exportwallet "filename"

Exports all wallet keys, for taddr and zaddr, in a human-readable format.  Overwriting an existing file is not permitted.

```
Arguments:
1. "filename"    (string, required) The filename, saved in folder set by pirated -exportdir option
```
```
Result:
"path"           (string) The full path of the destination file
```

### Examples:
```
pirate-cli z_exportwallet "test"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_exportwallet", "params": ["test"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
