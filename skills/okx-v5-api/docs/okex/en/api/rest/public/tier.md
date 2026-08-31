---
site: okex
lang: en
name: Get position tiers
method: GET
path: /api/v5/public/position-tiers
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get position tiers

Retrieve position tiers information, maximum leverage depends on your borrowings and Maintenance margin ratio.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/position-tiers`

> Request Example

```shell
GET /api/v5/public/position-tiers?tdMode=cross&instType=SWAP&instFamily=BTC-USDT
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve position tiers information
result = publicDataAPI.get_position_tiers(
    instType="SWAP",
    tdMode="cross",
    uly="BTC-USD"
)
print(result)
```

#### Request Parameters

| Parameter      | Type   | Required | Description                                                                                                                                                                                                                       |
|:---------------| :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType       | String | Yes       | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`                                                                                                                                                                    |
| tdMode         | String | Yes       | Trade mode<br>Margin mode `cross` `isolated`                                                                                                                                                                                      |
| instFamily     | String | Conditional  | Single instrument familiy or multiple instrument families (no more than 5) separated with comma.<br>If instType is `SWAP/FUTURES/OPTION`, `instFamily` is required.   |
| instId         | String | Conditional  | Single instrument or multiple instruments (no more than 5) separated with comma.<br>Either instId or ccy is required, if both are passed, instId will be used, ignore when instType is one of `SWAP`,`FUTURES`,`OPTION`           |
| ccy            | String | Conditional  | Margin currency<br>Only applicable to cross MARGIN. It will return borrowing amount for `Multi-currency margin` and `Portfolio margin` when `ccy` takes effect.                                                                  |
| tier           | String | No        | Tiers                                                                                                                                                                                                                             |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
    {
            "baseMaxLoan": "50",
            "imr": "0.1",
            "instId": "BTC-USDT",
            "maxLever": "10",
            "maxSz": "50",
            "minSz": "0",
            "mmr": "0.03",
            "optMgnFactor": "0",
            "quoteMaxLoan": "500000",
            "tier": "1",
            "uly": "",
            "instFamily": ""
        }
  ]
}
```

#### Response Parameters

| **Parameter**    | **Type** | **Description**                                                                                                                                                                                                             |
|:-----------------| :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uly              | String   | Underlying<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                       |
| instFamily       | String   | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                |
| instId           | String   | Instrument ID                                                                                                                                                                                                               |
| tier             | String   | Tiers                                                                                                                                                                                                                       |
| minSz            | String   | The minimum borrowing amount or position of this gear is only applicable to margin/options/perpetual/delivery, the minimum position is 0 by default<br>It will return the minimum borrowing amount when `ccy` takes effect. |
| maxSz            | String   | The maximum borrowing amount or number of positions held in this position is only applicable to margin/options/perpetual/delivery<br>It will return the maximum borrowing amount when `ccy` takes effect.                   |
| mmr              | String   | Position maintenance margin requirement rate                                                                                                                                                                                |
| imr              | String   | Initial margin requirement rate                                                                                                                                                                                             |
| maxLever         | String   | Maximum available leverage                                                                                                                                                                                                  |
| optMgnFactor     | String   | Option Margin Coefficient (only applicable to options)                                                                                                                                                                      |
| quoteMaxLoan     | String   | Quote currency borrowing amount (only applicable to leverage and the case when `instId` takes effect)                                                                                                                       |
| baseMaxLoan      | String   | Base currency borrowing amount (only applicable to leverage and the case when `instId` takes effect)                                                                                                                        |
