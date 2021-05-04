---
layout: default
title: z_sendmany
parent: RPC / CLI
nav_order: 26
---

## z_sendmany "fromaddress" [{"address":... ,"amount":...},...] ( minconf ) ( fee )

Send multiple times. Amounts are decimal numbers with at most 8 digits of precision.
Change generated from a taddr flows to a new taddr address, while change generated from a zaddr returns to itself.
When sending coinbase UTXOs to a zaddr, change is not allowed. The entire value of the UTXO(s) must be consumed.
Before Sapling activates, the maximum number of zaddr outputs is 54 due to transaction size limits.


```
Arguments:
1. "fromaddress"         (string, required) The taddr or zaddr to send the funds from.
2. "amounts"             (array, required) An array of json objects representing the amounts to send.
    [{
      "address":address  (string, required) The address is a taddr or zaddr
      "amount":amount    (numeric, required) The numeric amount in ARRR is the value
      "memo":memo        (string, optional) If the address is a zaddr, raw data represented in hexadecimal string format
    }, ... ]
3. minconf               (numeric, optional, default=1) Only use funds confirmed at least this many times.
4. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.
```
```
Result:
"operationid"          (string) An operationid to pass to z_getoperationstatus to get the result of the operation.
```

Examples:
```
pirate-cli z_sendmany "RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV" '[{"address": "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37" ,"amount": 5.0}]'
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_sendmany", "params": ["RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV", [{"address": "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37" ,"amount": 5.0}]] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
