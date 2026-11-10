---
site: okex
lang: zh
name: POST / 下单
method: POST
path: /api/v5/tradingBot/signal/sub-order
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 下单 [order-book-trading-signal-bot-trading-post-place-sub-order]

只有当您的账户有足够的资金才能下单。<br><br>

#### 限速：20次/2s

#### HTTP请求

`POST /api/v5/tradingBot/signal/sub-order`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/sub-order
body
{
    "algoId":"1222",
    "instId":"BTC-USDT-SWAP",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                      |
| :----- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId            | String   | 是       | 产品ID，如 `BTC-USDT-SWAP`                                                                                                                                            |
| algoId            | String   | 是       | 策略订单ID                                                                                                                    |
| side              | String   | 是       | 订单方向<br>`buy`：买，   `sell`：卖                                                                                                                                  |
| ordType           | String   | 是       | 订单类型  <br>`market`：市价单<br>`limit`：限价单                                                                                                                        |
| sz                | String   | 是       | 委托数量                                                                                                                                                              |
| px                | String   | 可选     | 委托价格，仅适用于`limit`                                                                                                                                               |
| reduceOnly        | Boolean  | 否       | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                    |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| code       | String   | 结果代码，`0`表示成功 |
| msg        | String   | 错误信息，代码为0时，该字段为空  |
| data       | Array of objects   | 包含结果的对象数组              |

<aside class="notice">
ordType<br>
订单类型，创建新订单时必须指定，您指定的订单类型将影响需要哪些订单参数和撮合系统如何执行您的订单，以下是有效的ordType：<br>
普通委托：<br>
limit：限价单，要求指定sz 和 px<br>
market：自动以最高买/最低卖价格委托，遵循限价机制<br>
</aside>

<aside class="notice">
sz 指合约张数。
</aside>

<aside class="notice">
reduceOnly<br>
只减仓，下单时，此参数设置为 true 时，表示此笔订单具有减仓属性，只会减少持仓数量，不会增加新的持仓仓位<br>
当前只减仓下单张数，加上价格时间优先于当前只减仓下单的只减仓挂单张数总和，不能超过持仓数量<br>
仅适用于`合约模式`和`跨币种保证金模式`<br>
</aside>
