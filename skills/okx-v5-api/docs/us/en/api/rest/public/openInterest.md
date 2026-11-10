---
site: us
lang: en
name: Get open interest
method: GET
path: /api/v5/public/open-interest
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Get open interest

Retrieve the total open interest for contracts on OKX.

#### Rate Limit: 20 requests per 2 seconds
#### Rate limit rule: IP + Instrument ID

#### HTTP Request

`GET /api/v5/public/open-interest`

> Request Example

```shell
GET /api/v5/public/open-interest?instType=SWAP
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve the total open interest for contracts on OKX
result = publicDataAPI.get_open_interest(
    instType="SWAP",
)
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required        | Description                                                                                                                    |
|:-----------| :----- |:----------------|:-------------------------------------------------------------------------------------------------------------------------------|
| instType   | String | Yes             | Instrument type<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS`                                                                           |
| instFamily | String | Conditional        | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`<br>If instType is `OPTION`, instFamily is required. |
| instId     | String | No              | Instrument ID, e.g. `BTC-USDT-SWAP`<br>Applicable to `FUTURES`/`SWAP`/`OPTION`/`EVENTS`                                                    |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
        "instType":"SWAP",
        "instId":"BTC-USDT-SWAP",
        "oi":"5000",
        "oiCcy":"555.55",
        "oiUsd": "50000",
        "ts":"1597026383085"
    }
  ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instType   | String   | Instrument type |
| instId      | String   | Instrument ID                                 |
| oi         | String   | Open interest in number of contracts                   |
| oiCcy      | String   | Open interest in number of coin             |
| oiUsd | String | Open interest in number of USD |
| ts         | String   | Data return time,  Unix timestamp format in milliseconds, e.g. `1597026383085` |
