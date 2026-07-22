---
site: okex
lang: zh
name: POST / 撤销策略委托订单
method: POST
path: /api/v5/trade/cancel-algos
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤销策略委托订单 [order-book-trading-algo-trading-post-cancel-algo-order]

撤销策略委托订单，每次最多可以撤销10个策略委托单

#### 限速：20个/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

#### HTTP请求

`POST /api/v5/trade/cancel-algos`

> 请求示例

```shell
POST /api/v5/trade/cancel-algos
body
[
    {
        "algoId":"590919993110396111",
        "instId":"BTC-USDT"
    },
    {
        "algoId":"590920138287841222",
        "instId":"BTC-USDT"
    }
]

```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 支持止盈止损，计划委托 类型的策略撤单
algo_orders = [
    {"instId": "BTC-USDT", "algoId": "590919993110396111"},
    {"instId": "BTC-USDT", "algoId": "590920138287841222"}
]

result = tradeAPI.cancel_algo_order(algo_orders)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                     |
| :----- | :----- | :------- | :----------------------- |
| instId | String | 是       | 产品ID 如 `BTC-USDT` |
| algoId | String | 可选       | 策略委托单ID<br>`algoId`和`algoClOrdId`必须传一个，若传两个，以`algoId`为主             |
| algoClOrdId | String | 可选       | 客户自定义策略订单ID<br>`algoId`和`algoClOrdId`必须传一个，若传两个，以`algoId`为主             |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "1836489397437468672",
            "clOrdId": "",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**                    |
|:------------| :------- |:--------------------------|
| algoId      | String   | 策略委托单ID                   |
| sCode       | String   | 事件执行结果的code，0代表成功         |
| sMsg        | String   | 事件执行失败时的msg               |
| clOrdId     | String   | <del>客户自定义订单ID</del>（已废弃） |
| algoClOrdId | String   | <del>客户自定义策略订单ID</del>（已废弃）               |
| tag         | String   | <del>订单标签</del>（已废弃）                      |
