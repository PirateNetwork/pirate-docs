---
layout: default
title: z_listunspent
parent: RPC / CLI
nav_order: 25
---

## z_listunspent ( minconf maxconf includeWatchonly ["zaddr",...] )

Returns array of unspent shielded notes with between minconf and maxconf (inclusive) confirmations.
Optionally filter to only include notes sent to specified addresses.
When minconf is 0, unspent notes with zero confirmations are returned, even though they are not immediately spendable.
Results are an array of Objects, each of which has:
{txid, jsindex, jsoutindex, confirmations, address, amount, memo} (Sprout)
{txid, outindex, confirmations, address, amount, memo} (Sapling)

```
Arguments:
1. minconf          (numeric, optional, default=1) The minimum confirmations to filter
2. maxconf          (numeric, optional, default=9999999) The maximum confirmations to filter
3. includeWatchonly (bool, optional, default=false) Also include watchonly addresses (see 'z_importviewingkey')
4. "addresses"      (string) A json array of zaddrs (Sapling Only) to filter on.  Duplicate addresses not allowed.
    [
      "address"     (string) zaddr
      ,...
    ]
```
```
Result
[                             (array of json object)
  {
    "txid" : "txid",          (string) the transaction id 
    "jsindex" : n             (numeric) the joinsplit index
    "jsoutindex" (sprout) : n          (numeric) the output index of the joinsplit
    "outindex" (sapling) : n          (numeric) the output index
    "confirmations" : n       (numeric) the number of confirmations
    "spendable" : true|false  (boolean) true if note can be spent by wallet, false if note has zero confirmations, false if address is watchonly
    "address" : "address",    (string) the shielded address
    "amount": xxxxx,          (numeric) the amount of value in the note
    "memo": xxxxx,            (string) hexademical string representation of memo field
    "change": true|false,     (boolean) true if the address that received the note is also one of the sending addresses
  }
  ,...
]
```

### Examples
```
pirate-cli z_listunspent 
```
```
pirate-cli z_listunspent 6 9999999 false "[\"zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37\",\"zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37\"]"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_listunspent", "params": [6,9999999,false,["zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37","zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"]] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
