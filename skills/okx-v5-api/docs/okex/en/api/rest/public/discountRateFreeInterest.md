---
site: okex
lang: en
name: Get discount rate and interest-free quota
method: GET
path: /api/v5/public/discount-rate-interest-free-quota
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get discount rate and interest-free quota

Retrieve discount rate level and interest-free quota.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/discount-rate-interest-free-quota`

> Request Example

```shell
GET /api/v5/public/discount-rate-interest-free-quota?ccy=BTC
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Retrieve discount rate level and interest-free quota
result = publicDataAPI.discount_interest_free_quota()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description |
| :-------- | :----- | :------- | :---------- |
| ccy       | String | No       | Currency    |
| discountLv        | String | No       | <del>Discount level (Deprecated) <del> |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0",
            "ccy": "BTC",
            "collateralRestrict": false,
            "details": [
                {
                    "discountRate": "0.98",
                    "liqPenaltyRate": "0.02",
                    "maxAmt": "20",
                    "minAmt": "0",
                    "tier": "1",
                    "disCcyEq": "1000"
                },
                {
                    "discountRate": "0.9775",
                    "liqPenaltyRate": "0.0225",
                    "maxAmt": "25",
                    "minAmt": "20",
                    "tier": "2",
                    "disCcyEq": "2000"
                }
            ],
            "discountLv": "1",
            "minDiscountRate": "0"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                        |
| :------------ | :------- |:---------------------------------------------------------------------------------------|
| ccy           | String   | Currency                                                                               |
| colRes               | String          | Platform level collateral restriction status<br> `0`: The restriction is not enabled.<br> `1`: The restriction is not enabled. But the crypto is close to the platform's collateral limit.<br> `2`: The restriction is enabled. This crypto can't be used as margin for your new orders. This may result in failed orders. But it will still be included in the account's adjusted equity and doesn't impact margin ratio.<br> Refer to [Introduction to the platform collateralized borrowing limit] (https://www.okx.com/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism) for more details. |
| collateralRestrict | Boolean | ~~Platform level collateralized borrow restriction<br> `true`<br> `false`~~(deprecated, use colRes instead) |
| amt           | String   | Interest-free quota                                                                    |
| discountLv         | String | <del>Discount rate level.(Deprecated)<del>                                             |
| minDiscountRate          | String   | Minimum discount rate when it exceeds the maximum amount of the last tier.             |
| details            | Array of objects  | New discount details.                                                                  |
| > discountRate     | String | Discount rate                                                                          |
| > maxAmt           | String | Tier - upper bound. <br/>The unit is the currency like BTC. "" means positive infinity |
| > minAmt           | String | Tier - lower bound. <br/>The unit is the currency like BTC. The minimum is 0           |
| > tier             | String | Tiers                                                                                  |
| > liqPenaltyRate   | String | Liquidation penalty rate                                                               |
| > disCcyEq | String | Discount equity in currency for quick calculation if your equity is the`maxAmt`        |
