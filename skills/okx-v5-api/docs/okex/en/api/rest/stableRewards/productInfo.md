---
site: okex
lang: en
name: GET / Product info
method: GET
path: /api/v5/finance/stable-rewards/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Product info

Retrieve product-level information for the specified stablecoin, including all currencies eligible for subscription and redemption, applicable fee rates, amount limits, daily quotas, and current redemption availability.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/stable-rewards/product-info`

> Request Example

```shell
GET /api/v5/finance/stable-rewards/product-info?ccy=USDG
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| ccy | String | Yes | Stablecoin, e.g. `USDG` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "details": [
                {
                    "ccy": "USDG",
                    "settleCcy": "USDC",
                    "subFeeRate": "0.0003",
                    "redemptFeeRate": "0",
                    "minSubAmt": "1",
                    "minRedeemAmt": "0.0000001",
                    "remainingSubQuota": "1000000",
                    "remainingRedemptQuota": "500000",
                    "canRedeem": true
                },
                {
                    "ccy": "USDG",
                    "settleCcy": "USDT",
                    "subFeeRate": "0.0003",
                    "redemptFeeRate": "",
                    "minSubAmt": "1",
                    "minRedeemAmt": "",
                    "remainingSubQuota": "1000000",
                    "remainingRedemptQuota": "",
                    "canRedeem": false
                }
            ],
            "ts": "1718035200000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| details | Array of objects | List of supported settlement currencies and their subscription/redemption details |
| > ccy | String | Subscribable stablecoin, e.g. `USDG` |
| > settleCcy | String | Settlement currency that can be used to subscribe to `ccy`, e.g. `USDC`, `USDT` |
| > subFeeRate | String | Subscription fee rate, e.g. `0.01` represents `1%` |
| > redemptFeeRate | String | Redemption fee rate, e.g. `0.01` represents `1%`<br>Returns `""` if redemption to the given `settleCcy` is not available |
| > minSubAmt | String | Minimum subscription amount, denominated in `settleCcy` |
| > minRedeemAmt | String | Minimum redemption amount, denominated in `ccy`<br>Returns `""` if redemption to the given `settleCcy` is not available |
| > remainingSubQuota | String | Remaining daily subscription quota per master user ID<br>Returns `-1` if unlimited |
| > remainingRedemptQuota | String | Remaining daily redemption quota per master user ID<br>Returns `-1` if unlimited<br>Returns `""` if redemption to the given `settleCcy` is not available |
| > canRedeem | Boolean | Whether redemption to the given `settleCcy` is currently available<br>`true`: Available<br>`false`: Unavailable |
| ts | String | Data query time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
