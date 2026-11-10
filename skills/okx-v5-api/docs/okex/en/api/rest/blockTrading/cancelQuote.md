---
site: okex
lang: en
name: Cancel Quote
method: POST
path: /api/v5/rfq/cancel-quote
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Cancel Quote

Cancels an existing active Quote you have created in response to an RFQ.

If a new `create-quote` for the same `rfqId` is processed before this cancel request arrives, the original quote will already be in `canceled` state and this request will return error `70400`. This can occur when requests are sent from different connections or processes, which do not guarantee ordering. To ensure strict create→cancel sequencing, wait for the create-quote response before issuing the cancel, using a single connection.

#### Rate Limit: 50 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/cancel-quote`

> Request Example

```shell
POST /api/v5/rfq/cancel-quote
{
    "quoteId": "007",
    "clQuoteId":"Bond007"
}
```
```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Cancel quote
result = blockTradingAPI.cancel_quote(
    quoteId="007",
    clQuoteId="Bond007"
)
print(result)
```

#### Request parameters
| Parameter  | Type  | Required | Description                                              |
| :------     | :----- | :-----   | :-----------------------------------------------------------  |
| quoteId   |String |Conditional |Quote ID.|
| clQuoteId |String |Conditional |Client-supplied Quote ID. Either `quoteId` or `clQuoteId` is required. If both `clQuoteId` and `quoteId` are passed, `quoteId` will be treated as primary identifier. |
| rfqId     |String |No          | RFQ ID.|

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "quoteId":"007",
            "clQuoteId":"Bond007",
            "sCode":"0",
            "sMsg":""
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
| > quoteId	|String	            |Quote ID                                             |
| > clQuoteId	|String	        |Client-supplied Quote ID.                            |
| > sCode	|String	            |The code of the event execution result, `0` means success.|
| > sMsg	|String	            |Rejection message if the request is unsuccessful.           |
