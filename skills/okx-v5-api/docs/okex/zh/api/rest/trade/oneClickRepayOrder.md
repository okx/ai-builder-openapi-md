---
site: okex
lang: zh
name: POST / 一键还债交易
method: POST
path: /api/v5/trade/one-click-repay
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 一键还债交易 [order-book-trading-trade-post-trade-one-click-repay]

交易一键偿还全仓债务。不支持逐仓负债的偿还。根据资金和交易账户的剩余可用余额为最大偿还数量。仅适用于`跨币种保证金模式`/`组合保证金模式`。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/trade/one-click-repay`

> 请求示例

```shell
POST /api/v5/trade/one-click-repay
body
{
    "debtCcy": ["ETH","BTC"], //逗号分隔债务币
    "repayCcy": "USDT" //用USDT偿还ETH和BTC
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 交易一键偿还小额全仓债务，使用USDT偿还ETH和BTC债务
result = tradeAPI.oneclick_repay(
    debtCcy=["ETH", "BTC"],
    repayCcy="USDT"
)
print(result)
```

#### 请求参数

| 参数名    | 类型              | 是否必须  | 描述                                                                 |
| :------- |:----------------| :------- | :-------------------------------------------------------------------- |
| debtCcy  | Array of strings | 是       | 负债币种 <br>单次最多同时选择5个币种，如有多个币种则用逗号隔开        |
| repayCcy | String          | 是       | 偿还币种 <br>只选择一个币种，且不能和负债币种重复               |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "debtCcy": "ETH", 
            "fillDebtSz": "0.01023052",
            "fillRepaySz": "30", 
            "repayCcy": "USDT", 
            "status": "filled",
            "uTime": "1646188520338"
        },
        {
            "debtCcy": "BTC", 
            "fillFromSz": "3",
            "fillToSz": "60,221.15910001",
            "repayCcy": "USDT",
            "status": "filled",
            "uTime": "1646188520338"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型**  | **描述**                                                                                 |
| :--------- | :------- | :--------------------------------------------------------------------------------------- |
| status     | String   | 当前还债进度/状态 <br>`running`: 进行中 <br>`filled`: 已完成 <br>`failed`: 失败             |
| debtCcy    | String   | 负债币种                                                                |
| repayCcy   | String   | 偿还币种                                                                      |
| fillDebtSz | String   | 负债币种成交数量                                                                |
| fillRepaySz| String   | 偿还币种成交数量                                                                      |
| uTime      | String   | 交易时间戳，Unix时间戳为毫秒数格式，如 1597026383085                                 |
