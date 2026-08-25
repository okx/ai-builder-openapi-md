---
site: okex
lang: en
name: Cancel multiple Quotes
method: POST
path: /api/v5/rfq/cancel-batch-quotes
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel multiple Quotes

Cancel one or multiple active Quote(s) in a single batch. Maximum 100 quote orders can be canceled per request.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/cancel-batch-quotes`

> Request Example

```shell
POST /api/v5/rfq/cancel-batch-quotes
{
    "quoteIds": ["1150","1151","1152"],
    "clQuoteIds": ["q1","q2","q3"]
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

# Cancel multiple quotes
result = blockTradingAPI.cancel_batch_quotes(
    quoteIds=["1150","1151","1152"],
    clQuoteIds=["q1","q2","q3"]
)
print(result)
```

#### Request parameters
| Parameter  | Type            | Required | Description                                              |
| :------     |:-----------------| :-----   | :-----------------------------------------------------------  |
| quoteIds   | Array of strings |Conditional | Quote IDs .|
| clQuoteIds | Array of strings |Conditional | Client-supplied Quote IDs. Either `quoteIds` or `clQuoteIds` is required.If both attributes are sent, `quoteIds` will be used as primary identifier.|

> Success - All requested Quotes canceled 

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1152",
            "clQuoteId":"q3",
            "sCode":"0",
            "sMsg":""
        }
    ]
}

```

> Partial cancellation 

```json
{
    "code":"2",
    "msg":"Bulk operation partially succeeded.",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1152",
            "clQuoteId":"q3",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        }
    ]
}

```

> Failure example

```json
{
    "code":"1",
    "msg":"Operation failed.",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q3",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
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
