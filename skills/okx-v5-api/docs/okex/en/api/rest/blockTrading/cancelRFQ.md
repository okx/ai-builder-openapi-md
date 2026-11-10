---
site: okex
lang: en
name: Cancel RFQ
method: POST
path: /api/v5/rfq/cancel-rfq
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Cancel RFQ

Cancel an existing active RFQ that you have created previously.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`POST /api/v5/rfq/cancel-rfq`

> Request Example

```shell
POST /api/v5/rfq/cancel-rfq
{
    "rfqId":"22535",
    "clRfqId":"rfq001"
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

# Cancel RFQ
result = blockTradingAPI.cancel_rfq(
    rfqId="22535",
    clRfqId="rfq001"
)
print(result)
```

#### Request parameters
| Parameter  | Type  | Required | Description                                              |
| :------     | :----- | :-----   | :-----------------------------------------------------------  |
|rfqId   |String |Conditional |RFQ ID created .|
|clRfqId |String |Conditional |Client-supplied RFQ ID. <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.  <br>Either rfqId or clRfqId is required. If both are passed, rfqId will be used.|

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "rfqId":"22535",
            "clRfqId":"rfq001",
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
| > rfqId	|String	            |RFQ ID                                             |
| > clRfqId	|String	            |Client-supplied RFQ ID.                            |
| > sCode	|String	            |The code of the event execution result, `0` means success.|
| > sMsg	|String	            |Rejection message if the request is unsuccessful.           |
