---
layout: default
title: z_listreceivedbyaddress
parent: RPC / CLI
nav_order: 24
---

## z_listreceivedbyaddress "address" ( minconf )

Return a list of amounts received by a zaddr belonging to the node’s wallet.

```
Arguments:
1. "address"      (string) The private address.
2. minconf          (numeric, optional, default=1) Only include transactions confirmed at least this many times.
```
```
Result:
{
  "txid": xxxxx,           (string) the transaction id
  "amount": xxxxx,         (numeric) the amount of value in the note
  "memo": xxxxx,           (string) hexadecimal string representation of memo field
  "confirmations" : n,     (numeric) the number of confirmations
  "jsindex" (sprout) : n,     (numeric) the joinsplit index
  "jsoutindex" (sprout) : n,     (numeric) the output index of the joinsplit
  "outindex" (sapling) : n,     (numeric) the output index
  "change": true|false,    (boolean) true if the address that received the note is also one of the sending addresses
}
```

### Examples:
```
pirate-cli z_listreceivedbyaddress "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_listreceivedbyaddress", "params": ["zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
