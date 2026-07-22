---
site: okex
lang: zh
name: POST / 调整保证金
method: POST
path: /api/v5/tradingBot/signal/margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 调整保证金 [order-book-trading-signal-bot-trading-post-adjust-margin-balance]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/signal/margin-balance`

> 请求示例

```shell
POST /api/v5/tradingBot/signal/margin-balance
body
{
   "algoId":"123456",
   "type":"add",
   "amt":"10"
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 是       | 策略ID                                                   |
| type      | String | 是       | 调整保证金类型<br>`add`：增加，`reduce`：减少  |
| amt       | String | 是       | 调整保证金数量  |
| allowReinvest | Boolean | 否  | 是否允许复投调整后的保证金，默认false。true 或者 false `false`:新投入的资金仅作为保证金用于避免爆仓<br>`true`:新投入的资金将可用于进行复投。<br>仅适用于进场设定为“TradingView 信号”或“初始投资比例”的策略  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "123456"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                      |
|:--------------| :------- | :---------------------------- |
| algoId        | String   | 策略ID                        |
