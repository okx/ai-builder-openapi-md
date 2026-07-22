---
site: okex
lang: zh
name: POST / 修改复投设置
method: POST
path: /api/v5/tradingBot/dca/settings/reinvestment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 修改复投设置 [order-book-trading-recurring-buy-post-amend-reinvestment]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/dca/settings/reinvestment`

> 请求示例

```shell
POST /api/v5/tradingBot/dca/settings/reinvestment
body
{
    "algoId": "2833925189933756416",
    "algoOrdType": "contract_dca",
    "allowReinvest": false
}
```
#### 请求参数
| 参数名            | 类型      | 是否必须 | 描述                              |
|:---------------|:--------|:------|:--------------------------------|
| algoId         | String  | 是     | 策略订单 ID                         |
| algoOrdType    | String  | 是     | 策略订单类型<br>`contract_dca`：合约马丁委托 |
| allowReinvest  | Boolean | 是     | 是否复投利润<br>`true` 或 `false`      |

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
| **参数名**       | **类型** | **描述**                          |
|:--------------|:-------|:--------------------------------|
| algoId        | String | 策略订单 ID                         |
| algoOrdType   | String | 策略订单类型<br>`contract_dca`：合约马丁委托 |
| sCode         | String | 事件执行结果的 code，0 代表成功             |
| sMsg          | String | 事件执行失败时的 msg                    |
