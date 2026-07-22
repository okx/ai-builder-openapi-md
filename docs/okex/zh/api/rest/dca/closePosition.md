---
site: okex
lang: zh
name: POST / 停止马丁策略委托订单
method: POST
path: /api/v5/tradingBot/dca/stop
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 停止马丁策略委托订单 [order-book-trading-dca-trading-post-close-position-for-contract-dca]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/dca/stop`

> 请求示例

```shell
POST /api/v5/tradingBot/dca/stop
body
{
    "algoOrdType": "contract_dca",
    "algoId": "448965992920907776",
    "stopType": "1"
}
```

#### 请求参数
| 参数名      | 类型   | 是否必须 | 描述                                                                                                                                                                              |
| :---------- | :----- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| algoId      | String | 是       | 策略ID                                                                                                                                                                            |
| algoOrdType | String | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托                                                                                                           |
| stopType    | String | 是       | 停止类型<br>合约马丁：`1`：市价全平，`2`：停止但不平仓<br>现货马丁：`1`：停止并卖出币，`2`：停止但不卖出币                                                                        |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "448965992920907776",
            "sCode": "0",
            "sMsg": ""
        }
    ]
}
```

#### 返回参数
| 参数名      | 类型   | 描述                              |
| :---------- | :----- | :-------------------------------- |
| algoId      | String | 策略ID                            |
| tag         | String | 订单标签                          |
| algoClOrdId | String | 客户端自定义策略单ID              |
| sCode       | String | 事件执行结果的code，0代表成功     |
| sMsg        | String | 事件执行失败时的msg               |
