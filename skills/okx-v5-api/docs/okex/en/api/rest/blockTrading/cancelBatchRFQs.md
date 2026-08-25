---
site: okex
lang: en
name: Cancel multiple RFQs
method: POST
path: /api/v5/rfq/cancel-batch-rfqs
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Cancel multiple RFQs

Cancel one or multiple active RFQ(s) in a single batch. Maximum 100 RFQ orders can be canceled per request.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/cancel-batch-rfqs`

> Request Example

```shell
POST /api/v5/rfq/cancel-batch-rfqs
{
    "rfqIds":[
        "2201",
        "2202",
        "2203"
    ],
    "clRfqIds":[
        "r1",
        "r2",
        "r3"
    ]
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

# Cancel multiple RFQs
result = blockTradingAPI.cancel_batch_rfqs(
    rfqIds=[
        "2201",
        "2202",
        "2203"
    ],
    clRfqIds=[
        "r1",
        "r2",
        "r3"
    ],
)
print(result)

```

#### Request parameters
| Parameter  | Type            | Required | Description                                              |
| :------     |:-----------------| :-----   | :-----------------------------------------------------------  |
|rfqIds   | Array of strings |Conditional |RFQ IDs .|
|clRfqIds | Array of strings |Conditional |Client-supplied RFQ IDs. <br>Either `rfqIds` or `clRfqIds` is required. <br>If both attributes are sent, `rfqIds` will be used as primary identifier.|

> Success - All requested RFQs canceled 

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "rfqId":"2201",
            "clRfqId":"r1",
            "sCode":"0",
            "sMsg":""
        },
        {
            "rfqId":"2202",
            "clRfqId":"r2",
            "sCode":"0",
            "sMsg":""
        },
        {
            "rfqId":"2203",
            "clRfqId":"r3",
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
    "msg":"Bulk operation partially ",
    "data":[
        {
            "rfqId":"2201",
            "clRfqId":"r1",
            "sCode":"70000",
            "sMsg":"RFQ does not exist."
        },
        {
            "rfqId":"2202",
            "clRfqId":"r2",
            "sCode":"0",
            "sMsg":""
        },
        {
            "rfqId":"2203",
            "clRfqId":"r3",
            "sCode":"0",
            "sMsg":""
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
            "rfqId":"2201",
            "clRfqId":"r1",
            "sCode":"70000",
            "sMsg":"RFQ does not exist."
        },
        {
            "rfqId":"2202",
            "clRfqId":"r2",
            "sCode":"70000",
            "sMsg":"RFQ does not exist."
        },
        {
            "rfqId":"2203",
            "clRfqId":"r3",
            "sCode":"70000",
            "sMsg":"RFQ does not exist."
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
| > rfqId	|String	            |RFQ ID                                             |
| > clRfqId	|String	            |Client-supplied RFQ ID.                            |
| > sCode	|String	            |The code of the event execution result, `0` means success.|
| > sMsg	|String	            |Rejection message if the request is unsuccessful.           |
