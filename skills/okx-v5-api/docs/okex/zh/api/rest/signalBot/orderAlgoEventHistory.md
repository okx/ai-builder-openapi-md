---
site: okex
lang: zh
name: GET / 获取信号策略历史事件
method: GET
path: /api/v5/tradingBot/signal/event-history
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取信号策略历史事件 [order-book-trading-signal-bot-trading-get-signal-bot-event-history]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/tradingBot/signal/event-history`

> 请求示例

```shell
GET /api/v5/tradingBot/signal/event-history?algoId=623833708424069120
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                      |
| :-------- | :----- |:-----|:------------------------------------------------------------------------|
| algoId    | String | 是    | 策略ID                                                                  |
| after    | String | 否    | 请求`eventCtime`在此时间之前（更旧的数据）的分页内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| before    | String | 否    | 请求`eventCtime`此时间之后（更新的数据）的分页内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`              |
| limit    | String | 否    | 返回结果的数量，最大为100，默认100条                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "alertMsg": "{\"marketPosition\":\"short\",\"prevMarketPosition\":\"long\",\"action\":\"sell\",\"instrument\":\"ETHUSDT.P\",\"timestamp\":\"2023-10-16T10:50:00.000Z\",\"maxLag\":\"60\",\"investmentType\":\"base\",\"amount\":\"2\"}",
            "algoId": "623833708424069120",
            "eventCtime": "1697453400959",
            "eventProcessMsg": "Processed reverse entry signal and placed ETH-USDT-SWAP order with all available balance",
            "eventStatus": "success",
            "eventType": "signal_processing",
            "eventUtime": "",
            "triggeredOrdData": [
                {
                    "clOrdId": "O634100754731765763"
                },
                {
                    "clOrdId": "O634100754752737282"
                }
            ]
        }
     ],
     "msg": ""
}
```

#### 返回参数

| **参数名**           | **类型**                   | **描述**                                                                         |
|:------------------|:-------------------------|:-------------------------------------------------------------------------------|
| alertMsg            | String                   | 提示信息                                                                           |
| algoId            | String                   | 策略ID                                                                           |
| eventType         | String                   | 事件类型<br>`system_action`：系统行为<br>`user_action`：用户行为<br>`signal_processing`：信号下单 |
| eventCtime        | String                   | 事件发生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                         |
| eventUtime        | String                   | 事件更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                         |
| eventProcessMsg   | String                   | 事件处理信息                                                                         |
| eventStatus       | String                   | 事件处理状态<br>`success`：成功<br>`failure`：失败                                         |
| triggeredOrdData  | Array of objects          | 信号触发的子订单的信息                                                                    |
| > clOrdId         | String                   | 子订单自定义ID                                                                       |
