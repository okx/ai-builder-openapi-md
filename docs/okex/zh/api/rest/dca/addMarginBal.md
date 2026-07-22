---
site: okex
lang: zh
name: POST / 增加保证金
method: POST
path: /api/v5/tradingBot/dca/margin/add
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 增加保证金 [order-book-trading-recurring-buy-post-add-margin]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/tradingBot/dca/margin/add`

> 请求示例

```shell
POST /api/v5/tradingBot/dca/margin/add
body
{
    "algoId": "2833925189933756416",
    "amt": "50"
}
```

#### 请求参数
| 参数名 | 类型 | 是否必须 | 描述 |
| :--- | :--- | :--- | :--- |
| algoId | String | 是 | 策略订单 ID |
| amt | String | 是 | 增加的保证金金额 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoOrdType": "contract_dca",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### 返回参数
| 参数名 | 类型 | 描述 |
| :--- | :--- | :--- |
| algoId | String | 策略订单 ID |
| algoOrdType | String | 策略订单类型<br>`contract_dca`：合约马丁委托 |
| sCode | String | 事件执行结果的 code，0 代表成功 |
| sMsg | String | 事件执行失败时的 msg |
