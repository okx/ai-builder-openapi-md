---
site: okex
lang: zh
name: GET / RSI回测（公共）
method: GET
path: /api/v5/tradingBot/public/rsi-back-testing
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / RSI回测（公共） [order-book-trading-grid-trading-get-rsi-back-testing-public]

公共接口无须鉴权

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/tradingBot/public/rsi-back-testing`

> 请求示例

```shell
GET /api/v5/tradingBot/public/rsi-back-testing?instId=BTC-USDT&thold=30&timeframe=3m&timePeriod=14
```

#### 请求参数

| 参数名         | 类型     | 是否必须 | 描述                                                                                                  |
|:------------|:-------|:-----|:----------------------------------------------------------------------------------------------------|
| instId      | String | 是    | 产品ID，如`BTC-USDT`<br>适用于`币币`                                                                         |
| timeframe   | String | 是    | K线种类<br>`3m`, `5m`, `15m`, `30m` (`m`代表分钟)<br>`1H`, `4H` (`H`代表小时)<br>`1D` (`D`代表天)                 |
| thold       | String | 是    | 阈值<br>取值[1,100]的整数                                               |
| timePeriod  | String | 是     | 周期<br>`14`                                                      |
| triggerCond | String | 否    | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>默认是`cross_down` |
| duration    | String | 否    | 回测周期<br>`1M`：1个月<br>默认`1M`                                                                          |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "triggerNum": "164"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述** |
|:------------| :------- |:-------|
| triggerNum  | String   | 触发次数   |
