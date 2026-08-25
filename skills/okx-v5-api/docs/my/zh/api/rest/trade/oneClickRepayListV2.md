---
site: my
lang: zh
name: GET / 获取一键还债币种列表(新)
method: GET
path: /api/v5/trade/one-click-repay-currency-list-v2
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取一键还债币种列表(新) [order-book-trading-trade-get-one-click-repay-currency-list-new]

查询一键还债币种列表。仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/trade/one-click-repay-currency-list-v2`

> 请求示例

```shell
GET /api/v5/trade/one-click-repay-currency-list-v2
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag,debug=True) 
result = tradeAPI.get_oneclick_repay_list_v2()
print(result)
```

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtData": [
                {
                    "debtAmt": "100",
                    "debtCcy": "USDC"
                }
            ],
            "repayData": [
                {
                    "repayAmt": "1.000022977",
                    "repayCcy": "BTC"
                },
                {
                    "repayAmt": "4998.0002397",
                    "repayCcy": "USDT"
                },
                {
                    "repayAmt": "100",
                    "repayCcy": "OKB"
                },
                {
                    "repayAmt": "1",
                    "repayCcy": "ETH"
                },
                {
                    "repayAmt": "100",
                    "repayCcy": "USDC"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**          | **描述**                                                                                 |
| :--------- |:----------------| :--------------------------------------------------------------------------------------- |
| debtData   | Array of objects | 负债币种信息                                                                         |
| > debtCcy   | String          | 负债币种                                                                                 |
| > debtAmt   | String          | 可负债币种数量<br>包括本金和利息                                                             |
| repayData  | Array of objects | 偿还币种信息                                                                      |
| > repayCcy  | String          | 可偿还负债的币种                                                                      |
| > repayAmt  | String          | 可偿还负债的币种可用资产数量                                                                 |
