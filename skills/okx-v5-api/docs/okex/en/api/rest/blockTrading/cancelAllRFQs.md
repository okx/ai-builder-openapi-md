---
site: okex
lang: en
name: Cancel all RFQs
method: POST
path: /api/v5/rfq/cancel-all-rfqs
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel all RFQs

Cancels all active RFQs.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/cancel-all-rfqs`

> Request Example

```shell
POST /api/v5/rfq/cancel-all-rfqs

```
```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Cancel all RFQs
result = blockTradingAPI.cancel_all_rfqs()
print(result)

```

#### Request parameters
None

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ts":"1697026383085"
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                              |
| :------ | :----- | :-----------------------------------------------------------  |
| code	    |String	            |The result code, `0` means success.|
| msg	    |String	            |The error message, not empty if the code is not 0.|
| data	    |Array of objects   |Array of objects containing the results            |
| > ts	|String	            |The timestamp of successful cancellation. Unix timestamp format in milliseconds, e.g. 1597026383085.  |
