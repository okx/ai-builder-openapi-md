---
site: okex
lang: zh
name: POST / 编辑定投周期
method: POST
path: /api/v5/tradingBot/recurring/amend-recurring-time
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 编辑定投周期 [order-book-trading-recurring-buy-post-amend-recurring-time]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`POST /api/v5/tradingBot/recurring/amend-recurring-time`

> 请求示例

```shell
POST /api/v5/tradingBot/recurring/amend-recurring-time
body
{
    "algoId": "2837428373700509696",
    "recurringTimeType": "1",
    "period": "hourly",
    "recurringHour": "8",
    "recurringDay": "1",
    "recurringTime": "11",
    "timeZone": "8"
}
```

#### 请求参数
| 参数名              | 类型   | 是否必须 | 描述                                                                                         |
| :------------------ | :----- | :------- | :------------------------------------------------------------------------------------------- |
| algoId              | String | 是       | 策略订单ID                                                                                    |
| recurringTimeType   | String | 是       | 定投周期类型<br>`1`：自定义时间<br>`2`：立即触发                                              |
| timeZone            | String | 是       | 时区（UTC），取值范围是 [-12,14] 的整数<br>如 `8` 表示UTC+8（东8区），北京时间               |
| period              | String | 是       | 周期类型<br>`monthly`：月<br>`weekly`：周<br>`daily`：日<br>`hourly`：小时                   |
| recurringHour       | String | 可选     | 小时级别定投的间隔<br>`1`/`4`/`8`/`12`<br>如：`1` 代表每隔 `1` 个小时定投<br>当 `period` 为 `hourly` 时必填 |
| recurringDay        | String | 可选     | 投资日<br>当周期类型为 `monthly`，则取值范围是 [1,28] 的整数<br>当周期类型为 `weekly`，则取值范围是 [1,7] 的整数<br>当周期类型为 `daily`/`hourly`，该参数可不填<br>仅在 `recurringTimeType` 为 `1` 时需要传 |
| recurringTime       | String | 可选     | 投资时间，取值范围是 [0,23] 的整数<br>仅在 `recurringTimeType` 为 `1` 时需要传               |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
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
