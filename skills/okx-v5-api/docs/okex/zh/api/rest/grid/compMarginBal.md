---
site: okex
lang: zh
name: POST / 调整保证金计算
method: POST
path: /api/v5/tradingBot/grid/compute-margin-balance
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 调整保证金计算 [order-book-trading-grid-trading-post-compute-margin-balance]

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/tradingBot/grid/compute-margin-balance`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/compute-margin-balance
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
| amt | String | 否       | 调整保证金数量<br>|

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "lever": "0.3877200981166066",
            "maxAmt": "1.8309562403342999"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| maxAmt      | String   | 最多可调整的保证金数量                        |
| lever      | String   | 调整保证金后的杠杠倍数                        |
