---
site: okex
lang: zh
name: GET / 查询限额
method: GET
path: /api/v5/finance/okusd/limits
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 查询限额 [financial-product-okusd-get-okusd-limits]

查询您当日 OKUSD 申购剩余限额及即时/标准赎回剩余限额。所有限额均以母账户维度计算，子账户共享。

#### 限速：2次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/okusd/limits`

> 请求示例

```shell
GET /api/v5/finance/okusd/limits
```

#### 请求参数

无

> 返回结果

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

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| subLimit | Object | 申购限额信息 |
| > maxSubAmt | String | 当日最大可申购数量（USDT），= min(personalDailyLimit - personalUsedAmt, platformDailyLimit - platformUsedAmt)，最小值为 `"0"` |
| > personalDailyLimit | String | 根据您的 VIP 等级对应的每日申购上限（USDT） |
| > personalUsedAmt | String | 您当日已申购金额（USDT） |
| > platformDailyLimit | String | 平台每日申购总上限（USDT） |
| > platformUsedAmt | String | 平台当日已申购总金额（USDT） |
| fastRedeemLimit | Object | 即时赎回限额信息（实时到账） |
| > personalDailyLimit | String | 根据您的 VIP 等级对应的每日即时赎回上限（OKUSD） |
| > personalUsedAmt | String | 您当日已使用的即时赎回额度（OKUSD） |
| > platformDailyLimit | String | 平台每日即时赎回总上限（OKUSD） |
| > platformUsedAmt | String | 平台当日已使用的即时赎回额度（OKUSD） |
| > feeRate | String | 即时赎回手续费率 |
| stdRedeemLimit | Object | 标准赎回限额信息 |
| > personalDailyLimit | String | 根据您的 VIP 等级对应的每日标准赎回上限（OKUSD） |
| > personalUsedAmt | String | 您当日已使用的标准赎回额度（OKUSD） |
| > platformDailyLimit | String | 平台每日标准赎回总上限（OKUSD） |
| > platformUsedAmt | String | 平台当日已使用的标准赎回额度（OKUSD） |
| > feeRate | String | 标准赎回手续费率 |
| ts | String | 服务器时间戳，Unix 时间戳，单位为毫秒，如 `1597026383085` |
