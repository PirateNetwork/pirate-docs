---
layout: default
title: z_mergetoaddress
parent: RPC / CLI
nav_order: 26
---

## z_mergetoaddress ["fromaddress", ... ] "toaddress" ( fee ) ( transparent_limit ) ( shielded_limit ) ( memo )

Merge multiple UTXOs and notes into a single UTXO or note.  Coinbase UTXOs are ignored; use `z_shieldcoinbase`
to combine those into a single note.

This is an asynchronous operation, and UTXOs selected for merging will be locked.  If there is an error, they
are unlocked.  The RPC call `listlockunspent` can be used to return a list of locked UTXOs.

The number of UTXOs and notes selected for merging can be limited by the caller.  If the transparent limit
parameter is set to zero, and Overwinter is not yet active, the -mempooltxinputlimit option will determine the
number of UTXOs.  Any limit is constrained by the consensus rule defining a maximum transaction size of
100000 bytes before Sapling, and 200000 bytes once Sapling activates.

```
Arguments:
1. fromaddresses         (string, required) A JSON array with addresses.
                         The following special strings are accepted inside the array:
                             - "*": Merge both UTXOs and notes from all addresses belonging to the wallet.
                             - "ANY_TADDR": Merge UTXOs from all t-addrs belonging to the wallet.
                             - "ANY_ZADDR": Merge notes from all z-addrs belonging to the wallet.
                         If a special string is given, any given addresses of that type will be ignored.
    [
      "address"          (string) Can be a t-addr or a z-addr
      ,...
    ]
2. "toaddress"           (string, required) The t-addr or z-addr to send the funds to.
3. fee                   (numeric, optional, default=0.0001) The fee amount to attach to this transaction.
4. transparent_limit     (numeric, optional, default=50) Limit on the maximum number of UTXOs to merge.  Set to 0 to use node option -mempooltxinputlimit (before Overwinter), or as many as will fit in the transaction (after Overwinter).
4. shielded_limit        (numeric, optional, default=10 Sprout or 90 Sapling Notes) Limit on the maximum number of notes to merge.  Set to 0 to merge as many as will fit in the transaction.
5. maximum_utxo_size       (numeric, optional) eg, 0.0001 anything under 10000 satoshies will be merged, ignores 10,000 sat p2pk utxo that iguana uses, and merges coinbase utxo.
6. "memo"                (string, optional) Encoded as hex. When toaddress is a z-addr, this will be stored in the memo field of the new note.
```
```
Result:
{
  "remainingUTXOs": xxx               (numeric) Number of UTXOs still available for merging.
  "remainingTransparentValue": xxx    (numeric) Value of UTXOs still available for merging.
  "remainingNotes": xxx               (numeric) Number of notes still available for merging.
  "remainingShieldedValue": xxx       (numeric) Value of notes still available for merging.
  "mergingUTXOs": xxx                 (numeric) Number of UTXOs being merged.
  "mergingTransparentValue": xxx      (numeric) Value of UTXOs being merged.
  "mergingNotes": xxx                 (numeric) Number of notes being merged.
  "mergingShieldedValue": xxx         (numeric) Value of notes being merged.
  "opid": xxx          (string) An operationid to pass to z_getoperationstatus to get the result of the operation.
}
```

### Examples:
```
pirate-cli z_mergetoaddress '["RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV"]' ztfaW34Gj9FrnGUEf833ywDVL62NWXBM81u6EQnM6VR45eYnXhwztecW1SjxA7JrmAXKJhxhj3vDNEpVCQoSvVoSpmbhtjf
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_mergetoaddress", "params": [["RD6GgnrMpPaTSMn8vai6yiGA7mN4QGPV"], "ztfaW34Gj9FrnGUEf833ywDVL62NWXBM81u6EQnM6VR45eYnXhwztecW1SjxA7JrmAXKJhxhj3vDNEpVCQoSvVoSpmbhtjf"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
