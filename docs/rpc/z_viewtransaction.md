---
layout: default
title: z_viewtransaction
parent: RPC / CLI
nav_order: 29
---

## z_viewtransaction "txid"

Get detailed shielded information about in-wallet transaction <txid>

```
Arguments:
1. "txid" (string, required) The transaction id
```
```
Result:
{
  "txid" : "transactionid",   (string) The transaction id
  "spends" : [
    {
      "type" : "sprout|sapling",      (string) The type of address
      "js" : n,                       (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsSpend" : n,                  (numeric, sprout) the index of the spend within the JSDescription
      "spend" : n,                    (numeric, sapling) the index of the spend within vShieldedSpend
      "txidPrev" : "transactionid",   (string) The id for the transaction this note was created in
      "jsPrev" : n,                   (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsOutputPrev" : n,             (numeric, sprout) the index of the output within the JSDescription
      "outputPrev" : n,               (numeric, sapling) the index of the output within the vShieldedOutput
      "address" : "zcashaddress",     (string) The Zcash address involved in the transaction
      "value" : x.xxx                 (numeric) The amount in ARRR
      "valueZat" : xxxx               (numeric) The amount in arrrtoshis
    }
    ,...
  ],
  "outputs" : [
    {
      "type" : "sprout|sapling",      (string) The type of address
      "js" : n,                       (numeric, sprout) the index of the JSDescription within vJoinSplit
      "jsOutput" : n,                 (numeric, sprout) the index of the output within the JSDescription
      "output" : n,                   (numeric, sapling) the index of the output within the vShieldedOutput
      "address" : "zcashaddress",     (string) The Zcash address involved in the transaction
      "recovered" : true|false        (boolean, sapling) True if the output is not for an address in the wallet
      "value" : x.xxx                 (numeric) The amount in ARRR
      "valueZat" : xxxx               (numeric) The amount in arrrtoshis
      "memo" : "hexmemo",             (string) Hexademical string representation of the memo field
      "memoStr" : "memo",             (string) Only returned if memo contains valid UTF-8 text.
    }
    ,...
  ],
}
```

### Examples:
```
pirate-cli z_viewtransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"
```
```
pirate-cli z_viewtransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d" true
```
```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "z_viewtransaction", "params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
