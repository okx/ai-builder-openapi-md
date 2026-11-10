---
site: my
lang: zh
name: POST / 一键还债交易(新)
method: POST
path: /api/v5/trade/one-click-repay-v2
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### POST / 一键还债交易(新) [order-book-trading-trade-post-trade-one-click-repay-new]

交易一键偿还债务。仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/trade/one-click-repay-v2`

> 请求示例

```shell
POST /api/v5/trade/one-click-repay-v2
body
{
    "debtCcy": "USDC", 
    "repayCcyList": ["USDC","BTC"] 
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"
flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag,debug=True)
result = tradeAPI.oneclick_repay_v2("USDC",["USDC","BTC"])
print(result)
```

#### 请求参数

| 参数名          | 类型              | 是否必须  | 描述                                                      |
|:-------------|:----------------| :------- |:--------------------------------------------------------|
| debtCcy      | String          | 是       | 负债币种                                                    |
| repayCcyList | Array of strings | 是       | 偿还币种列表，如 ["USDC","BTC"]<br>资产还币优先级和数组中的排序一致（排第一的优先级最高）。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "USDC",
            "repayCcyList": [
                "USDC",
                "BTC"
            ],
            "ts": "1742192217514"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**      | **类型**          | **描述**                                                      |
|:-------------|:----------------|:------------------------------------------------------------|
| debtCcy      | String          | 负债币种                                                        |
| repayCcyList | Array of strings | 偿还币种列表，如 ["USDC","BTC"]<br>资产还币优先级和数组中的排序一致（排第一的优先级最高）。     |
| ts           | String          | 请求时间，Unix时间戳为毫秒数格式，如 `1597026383085`                        |
