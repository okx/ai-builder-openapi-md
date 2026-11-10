---
site: okex
lang: zh
name: POST / 网格策略停止
method: POST
path: /api/v5/tradingBot/grid/stop-order-algo
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 网格策略停止 [order-book-trading-grid-trading-post-stop-grid-algo-order]

每次最多可以撤销10个网格策略。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/stop-order-algo`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/stop-order-algo
body
[
    {
        "algoId":"448965992920907776",
        "instId":"BTC-USDT",
        "stopType":"1",
        "algoOrdType":"grid"
    }
]
```

#### 请求参数

| 参数名           | 类型       | 是否必须 | 描述                                                         |
|:--------------|:---------| :------- | :----------------------------------------------------------- |
| algoId        | String   | 是       | 策略订单ID                                                   |
| instId        | String   | 是       | 产品ID，如`BTC-USDT`                                                   |
| algoOrdType   | String   | 是       | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托  |
| stopType      | String   | 是       | 网格策略停止类型<br>现货网格 `1`：卖出交易币，`2`：不卖出交易币<br>合约网格 `1`：市价全平 `2`：停止不平仓 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**                          |
|:------------| :------- |:--------------------------------|
| algoId      | String   | 策略订单ID                          |
| algoClOrdId | String   | 用户自定义策略ID                       |
| sCode       | String   | 事件执行结果的code，0代表成功               |
| sMsg        | String   | 事件执行失败时的msg                     |
| tag         | String   | 订单标签                            |
