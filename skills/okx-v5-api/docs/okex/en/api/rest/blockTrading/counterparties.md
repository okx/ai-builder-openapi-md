---
site: okex
lang: en
name: Get Counterparties
method: GET
path: /api/v5/rfq/counterparties
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get Counterparties

Retrieves the list of counterparties that the user is permitted to trade with. 

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/rfq/counterparties`

> Request Example

```shell
GET /api/v5/rfq/counterparties

```
```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Get counterparts
result = blockTradingAPI.counterparties()
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
            "traderName" : "Satoshi Nakamoto",
            "traderCode" : "SATOSHI",
            "type" : "" 
        }
    ]
}
```

#### Response Parameters

| Parameter  | Type   | Description                                                                                                                                                            |
| :------ | :----- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| traderName| String| The long formative username of trader or entity on the platform.                                                                                    |
| traderCode| String| A unique identifier of maker which will be publicly visible on the platform. All RFQ and Quote endpoints will use this as the unique counterparty identifier. |
| type		| String| The counterparty type. `LP` refers to API connected auto market makers.                                                                                           |
