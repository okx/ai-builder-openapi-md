---
site: okex
lang: en
name: GET / OKUSD limits
method: GET
path: /api/v5/finance/okusd/limits
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / OKUSD limits

Retrieve your remaining daily OKUSD subscription quota and both fast and standard redemption quotas. All limits are calculated at the master-account level and shared across sub-accounts.

#### Rate Limit: 2 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/finance/okusd/limits`

> Request Example

```shell
GET /api/v5/finance/okusd/limits
```

#### Request Parameters

None

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "subLimit": {
                "maxSubAmt": "45000000",
                "personalDailyLimit": "5000000",
                "personalUsedAmt": "500000",
                "platformDailyLimit": "50000000",
                "platformUsedAmt": "5000000"
            },
            "fastRedeemLimit": {
                "personalDailyLimit": "10000",
                "personalUsedAmt": "0",
                "platformDailyLimit": "5000000",
                "platformUsedAmt": "1000000",
                "feeRate": "0.001"
            },
            "stdRedeemLimit": {
                "personalDailyLimit": "1000000",
                "personalUsedAmt": "0",
                "platformDailyLimit": "40000000",
                "platformUsedAmt": "0",
                "feeRate": "0.00025"
            },
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| subLimit | Object | Subscription limit information |
| > maxSubAmt | String | Maximum subscribable amount for today (USDT). Equal to `min(personalDailyLimit - personalUsedAmt, platformDailyLimit - platformUsedAmt)`. Minimum value is `"0"` |
| > personalDailyLimit | String | Your daily subscription limit based on your VIP tier (USDT) |
| > personalUsedAmt | String | Amount you have already subscribed today (USDT) |
| > platformDailyLimit | String | Platform-wide daily subscription cap (USDT) |
| > platformUsedAmt | String | Total amount subscribed across the platform today (USDT) |
| fastRedeemLimit | Object | Fast redemption limit information (real-time settlement) |
| > personalDailyLimit | String | Your daily fast redemption limit based on your VIP tier (OKUSD) |
| > personalUsedAmt | String | Fast redemption amount you have already used today (OKUSD) |
| > platformDailyLimit | String | Platform-wide daily fast redemption cap (OKUSD) |
| > platformUsedAmt | String | Total fast redemption amount used across the platform today (OKUSD) |
| > feeRate | String | Fee rate for fast redemption |
| stdRedeemLimit | Object | Standard redemption limit information |
| > personalDailyLimit | String | Your daily standard redemption limit based on your VIP tier (OKUSD) |
| > personalUsedAmt | String | Standard redemption amount you have already used today (OKUSD) |
| > platformDailyLimit | String | Platform-wide daily standard redemption cap (OKUSD) |
| > platformUsedAmt | String | Total standard redemption amount used across the platform today (OKUSD) |
| > feeRate | String | Fee rate for standard redemption |
| ts | String | Server timestamp, Unix timestamp format in milliseconds, e.g. `1597026383085` |
