---
site: okex
lang: zh
name: GET / 获取一键还债币种列表
method: GET
path: /api/v5/trade/one-click-repay-currency-list
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取一键还债币种列表 [order-book-trading-trade-get-one-click-repay-currency-list]

查询一键还债币种列表。负债币种包括全仓负债和逐仓负债。仅适用于`跨币种保证金模式`/`组合保证金模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/one-click-repay-currency-list`

> 请求示例

```shell
GET /api/v5/trade/one-click-repay-currency-list
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 查询一键还债币种列表
result = tradeAPI.get_oneclick_repay_list()
print(result)
```

#### 请求参数

| 参数名    | 类型    | 是否必须  | 描述                                                                 |
| :------- | :----- | :------- | :-------------------------------------------------------------------- |
| debtType | String | 否        | 负债类型 <br>`cross`: 全仓负债 <br>`isolated`: 逐仓负债        |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtData": [
                {
                    "debtAmt": "29.653478",
                    "debtCcy": "LTC"
                },
                {
                    "debtAmt": "237803.6828295906051002",
                    "debtCcy": "USDT"
                }
            ],
            "debtType": "cross",
            "repayData": [
                {
                    "repayAmt": "0.4978335419825104",
                    "repayCcy": "ETH"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**  | **描述**                                                                                 |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| debtData   | Array of objects    | 负债币种信息                                                                         |
| > debtCcy   | String   | 负债币种                                                                                 |
| > debtAmt   | String   | 可负债币种数量<br>包括本金和利息                                                             |
| debtType   | String   | 负债类型 <br>`cross`: 全仓负债 <br>`isolated`: 逐仓负债                                  |
| repayData  | Array of objects    | 偿还币种信息                                                                      |
| > repayCcy  | String   | 可偿还负债的币种                                                                      |
| > repayAmt  | String   | 可偿还负债的币种可用资产数量                                                                 |
