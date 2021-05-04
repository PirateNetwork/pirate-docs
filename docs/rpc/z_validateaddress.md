---
layout: default
title: z_validateaddress
parent: RPC / CLI
nav_order: 9
---

## z_validateaddress "zaddr"

Return information about the given z address.

```
Arguments:
1. "zaddr"     (string, required) The z address to validate
```
```
Result:
{
  "isvalid" : true|false,      (boolean) If the address is valid or not. If not, this is the only property returned.
  "address" : "zaddr",         (string) The z address validated
  "type" : "xxxx",             (string) "sprout" or "sapling"
  "ismine" : true|false,       (boolean) If the address is yours or not
  "payingkey" : "hex",         (string) [sprout] The hex value of the paying key, a_pk
  "transmissionkey" : "hex",   (string) [sprout] The hex value of the transmission key, pk_enc
  "diversifier" : "hex",       (string) [sapling] The hex value of the diversifier, d
  "diversifiedtransmissionkey" : "hex", (string) [sapling] The hex value of pk_d
}
```

### Examples:
```pirate-cli z_validateaddress "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"```

```curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_validateaddress", "params": ["zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/```
