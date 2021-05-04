---
layout: default
title: zs_gettransaction
parent: RPC / CLI
nav_order: 2
---

## zs_gettransaction

Returns a decrypted Pirate transaction.

```
Arguments:
1. "txid:"   (string, required) 


Result:
   "txid":  "transactionid",           (string)  The transaction id.
   "coinbase": "coinbase",             (string)  Coinbase transaction, true or false
   "category": "category",             (string)  orphan (coinbase), immature (coinbase), generate (coinbase), regular
   "blockhash": "hashvalue",           (string)  The block hash containing the transaction
   "blockindex": n,                    (numeric) The block index containing the transaction
   "blocktime": n,                     (numeric) The block time in seconds of the block containing the transaction, 0 for unconfirmed transactions
   "rawconfirmations": n,              (numeric) The number of onchain confirmations for the transaction
   "confirmations": n,                 (numeric) The number of dpow confirmations for the transaction
   "time": xxx,                        (numeric) The transaction time in seconds of the transaction
   "expiryHeight": n,                  (numeric) The expiry height of the transaction
   "size": xxx,                        (numeric) The transaction size
   "fee": n,                           (numeric) Transaction fee in arrrtoshis
   "spends": {                       A list of the spends used as inputs in the transaction
      "type": "address type",          (string)  transparent, sprout, sapling
      "spend": n,                      (numeric) spend index
      "txidPrev":  "transactionid",    (string)  The transaction id of the output being spent
      "outputPrev": n,                 (numeric) vout, shieledoutput or jsindex of output being spent 
      "spendJsOutIndex": n,            (numeric) Joinsplit Output index of the output being spent (sprout address type only)
      "address": "address",            (string)  Pirate address
      "scriptPubKey": "script",        (string)  Script for the Pirate transparent address (transparent address type only)
      "value": x.xxxx,                 (numeric) Value of output being spent ARRR
      "valueZat": xxxxx,               (numeric) Value of output being spent in arrrtoshis ARRR
      "spendable": true/false          (bool)  Is this output spendable by this wallet
      },
   "sent": {                        A list of outputs of where funds were sent to in the transaction,
      "type": "address type",          (string)  transparent, sprout, sapling
      "output": n,                     (numeric) vout, shieledoutput or jsindex
      "outIndex": n,                   (numeric) Joinsplit Output index (sprout address type only)
      "outgoing": true or false,       (bool)    funds leaving the wallet
      "address": "address",            (string)  Pirate address
      "scriptPubKey": "script",        (string)  Script for the Pirate transparent address (transparent address type only)
      "value": x.xxxx,                 (numeric) Value of output being spent ARRR
      "valueZat": xxxxx,               (numeric) Value of output being spent in arrrtoshis ARRR
      "change": true/false             (string)  The note is change. This can result from sending funds
      "memo": "hex string",            (string)  Hex encoded memo (sprout and sapling address types only)
      "memoStr": "memo",               (string)  UTF-8 encoded memo (sprout and sapling address types only)
   }
   "recieved": {                     A list of receives from the transaction
      "type": "address type",          (string)  transparent, sprout, sapling
      "output": n,                     (numeric) vout, shieledoutput or jsindex
      "outIndex": n,                   (numeric) Joinsplit Output index (sprout address type only)
      "outgoing": true or false,       (bool)    funds leaving the wallet
      "address": "address",            (string)  Pirate address
      "scriptPubKey": "script",        (string)  Script for the Pirate transparent address (transparent address type only)
      "value": x.xxxx,                 (numeric) Value of output being spent ARRR
      "valueZat": xxxxx,               (numeric) Value of output being spent in arrrtoshis ARRR
      "change": true/false             (string)  The note is change. This can result from sending funds
      "spendable": true/false          (bool)  Is this output spendable by this wallet
      "memo": "hex string",            (string)  Hex encoded memo (sprout and sapling address types only)
      "memoStr": "memo",               (string)  UTF-8 encoded memo (sprout and sapling address types only)
   },
```

### Examples:
> pirate-cli zs_gettransaction "1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"

> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "zs_gettransaction", "params": ["1075db55d416d3ca199f55b6084e2115b9345e16c5cf302fc80e9d5fbf5d48d"] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
