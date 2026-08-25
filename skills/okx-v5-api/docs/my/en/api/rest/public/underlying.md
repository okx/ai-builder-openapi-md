---
site: my
lang: en
name: Get underlying
method: GET
path: /api/v5/public/underlying
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get underlying

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/public/underlying`

> Request Example

```shell
GET /api/v5/public/underlying?instType=FUTURES
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Get underlying
result = publicDataAPI.get_underlying(
    instType="FUTURES"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                   |
| :----- | :----- | :------- | :----------------------------------------------------- |
| instType | String | Yes       | Instrument type<br>`SWAP`<br>`FUTURES`<br>`OPTION` |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "LTC-USDT",
            "BTC-USDT",
            "ETC-USDT"
        ]
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                  |
| :--------- | :------- | :-------------------------------------------------------- |
| uly   | Array   | Underlying    |
