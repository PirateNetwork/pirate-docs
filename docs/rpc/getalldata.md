---
layout: default
title: getalldata
parent: RPC / CLI
nav_order: 1
---

## getalldata "datatype transactiontype"

This function only returns information on wallet addresses with full spending keys.

```
Arguments:
1. "datatype"     (integer, required) 
                    Value of 0: Return address, balance, transactions and blockchain info
                    Value of 1: Return address, balance, blockchain info
                    Value of 2: Return transactions and blockchain info
2. "transactiontype"     (integer, optional) 
                    Value of 0: Return all transactions
                    Value of 1: Return all transactions in the last 24 hours
                    Value of 2: Return all transactions in the last 7 days
                    Value of 3: Return all transactions in the last 30 days
                    Value of 4: Return all transactions in the last 90 days
                    Value of 5: Return all transactions in the last 365 days
                    Other number: Return all transactions
3. "transactioncount"     (integer, optional) 
4. "Include Watch Only"   (bool, optional, Default = false) 
```

### Examples:
```
pirate-cli getalldata 0
```

```
curl --user myusername --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getalldata", "params": [0] }' -H 'content-type: text/plain;' http://127.0.0.1:45453/
```


