---
site: okex
lang: zh
name: POST / 暂停定投策略
method: POST
path: /api/v5/tradingBot/recurring/pause
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 暂停定投策略 [order-book-trading-recurring-buy-post-pause]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/tradingBot/recurring/pause`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/pause
body
{
    "algoId": "2837428373700509696"
}
```

#### 请求参数
| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| algoId | String | 是 | 策略订单ID |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2837428373700509696",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### 返回参数
| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| algoId | String | 策略订单ID |
| sCode | String | 事件执行结果的 code，0 代表成功 |
| sMsg | String | 事件执行失败时的 msg |
