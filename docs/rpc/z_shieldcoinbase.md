---
layout: default
title: z_shieldcoinbase
parent: RPC / CLI
nav_order: 28
---

## z_shieldcoinbase "fromaddress" "tozaddress" ( fee ) ( limit )

Shield transparent coinbase funds by sending to a shielded zaddr.  This is an asynchronous operation and utxos
selected for shielding will be locked.  If there is an error, they are unlocked.  The RPC call `listlockunspent`
can be used to return a list of locked utxos.  The number of coinbase utxos selected for shielding can be limited
by the caller.  If the limit parameter is set to zero, and Overwinter is not yet active, the -mempooltxinputlimit
option will determine the number of uxtos.  Any limit is constrained by the consensus rule defining a maximum
transaction size of 100000 bytes before Sapling, and 200000 bytes once Sapling activates.

```
Arguments:
1. "fromaddress"         (string, required) The address is a taddr or "*" for all taddrs belonging to the wallet.
2. "toaddress"           (string, required) The address is a zaddr.
3. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.
4. limit                 (numeric, optional, default=50) Limit on the maximum number of utxos to shield.  Set to 0 to use node option -mempooltxinputlimit (before Overwinter), or as many as will fit in the transaction (after Overwinter).
```
```
Result:
{
  "remainingUTXOs": xxx       (numeric) Number of coinbase utxos still available for shielding.
  "remainingValue": xxx       (numeric) Value of coinbase utxos still available for shielding.
  "shieldingUTXOs": xxx        (numeric) Number of coinbase utxos being shielded.
  "shieldingValue": xxx        (numeric) Value of coinbase utxos being shielded.
  "opid": xxx          (string) An operationid to pass to z_getoperationstatus to get the result of the operation.
}
```

### Examples:
```
pirate-cli z_shieldcoinbase "RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV" "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_shieldcoinbase", "params": ["RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV", "zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
