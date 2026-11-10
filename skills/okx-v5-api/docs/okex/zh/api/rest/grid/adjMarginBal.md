---
site: okex
lang: zh
name: POST / 调整保证金
method: POST
path: /api/v5/tradingBot/grid/margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 调整保证金 [order-book-trading-grid-trading-post-adjust-margin-balance]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/margin-balance`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/margin-balance
body {
   "algoId":"123456",
   "type":"add",
   "amt":"10"
}
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| algoId    | String | 是       | 策略订单ID                                                   |
| type | String | 是       | 调整保证金类型<br>`add`：增加，`reduce`：减少  |
| amt | String | 可选       | 调整保证金数量<br>`amt`和`percent`必须传一个  |
| percent | String | 可选       | 调整保证金百分比  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoClOrdId": "",
            "algoId": "123456"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**       | **类型** | **描述**                      |
|:--------------| :------- | :---------------------------- |
| algoId        | String   | 策略订单ID                        |
| algoClOrdId   | String   | 用户自定义策略ID                        |
