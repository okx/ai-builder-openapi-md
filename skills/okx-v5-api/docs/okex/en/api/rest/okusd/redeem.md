---
site: okex
lang: en
name: POST / Redeem OKUSD
method: POST
path: /api/v5/finance/okusd/redeem
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / Redeem OKUSD

Redeem OKUSD back to USDT. Choose between fast redemption (real-time settlement) or standard redemption (D+5 or D+6 calendar days, depending on submission time). Fee rates vary by redemption type — call `GET /limits` to get current rates. All fee amounts are truncated (floor) to 8 decimal places. Pass a unique `clOrdId` per request; resubmitting the same `clOrdId` returns the original order without re-executing the redemption.

#### Rate Limit: 1 request per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/finance/okusd/redeem`

> Request Example (Fast Redemption)

```shell
POST /api/v5/finance/okusd/redeem
body
{
    "amt": "1000.00000000",
    "redeemType": "1",
    "clOrdId": "my-redeem-001"
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| amt | String | Yes | Amount of OKUSD to redeem. Minimum: `1`. Maximum 8 decimal places. Scientific notation is not supported |
| redeemType | String | Yes | Redemption type. `"1"`: Fast redemption (real-time settlement); `"2"`: Standard redemption (D+5 calendar days if submitted before UTC+8 16:00; D+6 calendar days if submitted at or after UTC+8 16:00). See the `feeRate` fields returned by `GET /limits` for current fee rates |
| clOrdId | String | Yes | Client-defined order ID. Maximum 32 characters (letters, digits, `-`, `_`). Must be unique per UID. Used for order tracking and idempotency |

> Response Example (Fast Redemption)

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678905678",
            "clOrdId": "my-redeem-001",
            "ccy": "OKUSD",
            "amt": "1000.00000000",
            "fee": "1.00000000",
            "usdtAmt": "999.00000000",
            "redeemType": "1",
            "state": "success",
            "estSettlementTime": "1718500010000",
            "ts": "1718500000000"
        }
    ]
}
```

> Response Example (Standard Redemption)

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "ordId": "680012345678906789",
            "clOrdId": "my-redeem-002",
            "ccy": "OKUSD",
            "amt": "50000.00000000",
            "fee": "12.50000000",
            "usdtAmt": "49987.50000000",
            "redeemType": "2",
            "state": "processing",
            "estSettlementTime": "1718932800000",
            "ts": "1718500000000"
        }
    ]
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description** |
| --- | --- | --- |
| ordId | String | System-generated order ID |
| clOrdId | String | Client-defined order ID (echoed back) |
| ccy | String | Redemption currency. Always `"OKUSD"` |
| amt | String | OKUSD amount redeemed |
| fee | String | Fee charged in USDT, truncated (floor) to 8 decimal places |
| usdtAmt | String | Net USDT amount credited to your funding account (`amt - fee`, truncated to 8 decimal places) |
| redeemType | String | Redemption type: `"1"` (fast) or `"2"` (standard) |
| state | String | Order status: `"processing"` / `"success"` / `"failed"` / `"cancelled"` |
| estSettlementTime | String | Estimated settlement time, Unix timestamp format in milliseconds. Fast redemption: current time. Standard redemption: D+5 calendar days if submitted before UTC+8 16:00; D+6 calendar days if submitted at or after UTC+8 16:00 |
| ts | String | Order creation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
