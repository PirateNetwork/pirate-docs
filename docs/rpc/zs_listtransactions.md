---
layout: default
title: zs_listtransactions
parent: RPC / CLI
nav_order: 6
---

## zs_listtransactions

Returns an array of decrypted Pirate transactions.

This function only returns information on addresses with full spending keys.

```
Arguments:
1. "Minimum Confimations:"   (numeric, optional, default=0) 

2. "Filter Type:"            (numeric, optional, default=0) 
                               Value of 0: Returns all transactions in the wallet
                               Value of 1: Returns the last x days of transactions
                               Value of 2: Returns transactions with confimations less than x
                               Value of 3: Returns transactions with a minimum block height of x

3. "Filter:"                 (numeric, optional, default=0) 
                               Filter Type equal 0: paramater ignored
                               Filter Type equal 1: number represents the number of days returned
                               Filter Type equal 2: number represents the max confirmations for transaction to be returned
                               Filter Type equal 3: number represents the minimum block height of the transactions returned

4. "Count:"                 (numeric, optional, default=100000) 
                               Last n number of transactions returned

5. "Include Watch Only"   (bool, optional, Default = false) 

Default Parameters:
1. 0 - O confimations required
2. 0 - Returns all transactions
3. 0 - Ignored
4. 100000 - Return the last 100,000 transactions.
5. false - exclude watch only
```
```
Result:
[{
                                     An Array of Transactions
   "txid":  "transactionid",           (string)  The transaction id.
   "coinbase": "coinbase",             (string)  Coinbase transaction, true or false
   "category": "category",             (string)  orphan (coinbase), immature (coinbase), generate (coinbase), regular
   "blockhash": "hashvalue",           (string)  The block hash containing the transaction
   "blockindex": n,                    (numeric) The block index containing the transaction
   "blocktime": n,                     (numeric) The block time in seconds of the block containing the transaction, 0 for unconfirmed transactions
   "expiryHeight": n,                  (numeric) The expiry height of the transaction
   "confirmations": n,                 (numeric) The number of confirmations for the transaction
   "time": xxx,                        (numeric) The transaction time in seconds of the transaction
   "size": xxx,                        (numeric) The transaction size
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
}]
```

### Examples:
```
pirate-cli zs_listtransactions
```

```
pirate-cli zs_listtransactions 1
```

```
pirate-cli zs_listtransactions 1 1 30 200
```

```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "zs_listtransactions", "params": [] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```

```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "zs_listtransactions", "params": [1] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```

```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "zs_listtransactions", "params": [1 1 30 200] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```
