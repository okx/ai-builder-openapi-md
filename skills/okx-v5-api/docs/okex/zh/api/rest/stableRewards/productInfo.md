---
site: okex
lang: zh
name: GET / 获取产品信息
method: GET
path: /api/v5/finance/stable-rewards/product-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取产品信息 [financial-product-stable-rewards-get-product-info]

获取指定稳定币的产品信息，包括支持订阅/赎回的结算币种、适用手续费率、申购/赎回金额限制、每日配额以及当前赎回可用状态。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/stable-rewards/product-info`

> 请求示例

```shell
GET /api/v5/finance/stable-rewards/product-info?ccy=USDG
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| ccy | String | 是 | 稳定币，如 `USDG` |

> 返回结果

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

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| details | Array of objects | 当前稳定币支持的结算币种及其订阅/赎回信息列表 |
| > ccy | String | 可订阅的稳定币，如 `USDG` |
| > settleCcy | String | 可用于订阅 `ccy` 的结算币种，如 `USDC`、`USDT` |
| > subFeeRate | String | 订阅手续费率，如 `0.01` 代表 `1%` |
| > redemptFeeRate | String | 赎回手续费率，如 `0.01` 代表 `1%`<br>当前 `settleCcy` 不支持赎回时返回 `""` |
| > minSubAmt | String | 最小订阅数量，以 `settleCcy` 计价 |
| > minRedeemAmt | String | 最小赎回数量，以 `ccy` 计价<br>当前 `settleCcy` 不支持赎回时返回 `""` |
| > remainingSubQuota | String | 每日剩余订阅额度，按母账户 ID 统计<br>`-1` 代表无上限 |
| > remainingRedemptQuota | String | 每日剩余赎回额度，按母账户 ID 统计<br>`-1` 代表无上限<br>当前 `settleCcy` 不支持赎回时返回 `""` |
| > canRedeem | Boolean | 当前 `settleCcy` 是否支持赎回<br>`true`：可赎回<br>`false`：不可赎回 |
| ts | String | 数据查询时间，Unix 时间戳，单位为毫秒，如 `1597026383085` |
