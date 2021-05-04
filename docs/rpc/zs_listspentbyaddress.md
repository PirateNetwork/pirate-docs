---
layout: default
title: 
parent: RPC / CLI
nav_order: 5
---

## zs_listspentbyaddress

Returns decrypted Pirate spent inputs for a single address.

This function only returns information on addresses with full spending keys.

```
Arguments:
1. "address:"                (string, required) 

2. "Minimum Confimations:"   (numeric, optional, default=0) 

3. "Filter Type:"            (numeric, optional, default=0) 
                               Value of 0: Returns all transactions in the wallet
                               Value of 1: Returns the last x days of transactions
                               Value of 2: Returns transactions with confimations less than x
                               Value of 3: Returns transactions with a minimum block height of x

4. "Filter:"                 (numeric, optional, default=0) 
                               Filter Type equal 0: paramater ignored
                               Filter Type equal 1: number represents the number of days returned
                               Filter Type equal 2: number represents the max confirmations for transaction to be returned
                               Filter Type equal 3: number represents the minimum block height of the transactions returned

5. "Count:"                 (numeric, optional, default=100000) 
                               Last n number of transactions returned

Default Parameters:
1. Pirate Address
2. 0 - O confimations required
3. 0 - Returns all transactions
4. 0 - Ignored
5. 100000 - Return the last 9,999,999 transactions.
```
```
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
```

### Examples:
> pirate-cli zs_listspentbyaddress zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37

> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "zs_listspentbyaddress", "params": [zs14d8tc0hl9q0vg5l28uec5vk6sk34fkj2n8s7jalvw5fxpy6v39yn4s2ga082lymrkjk0x2nqg37] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
