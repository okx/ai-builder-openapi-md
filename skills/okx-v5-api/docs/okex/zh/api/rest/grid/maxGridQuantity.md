---
site: okex
lang: zh
name: GET / 最大网格数量（公共）
method: GET
path: /api/v5/tradingBot/grid/grid-quantity
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 最大网格数量（公共） [order-book-trading-grid-trading-get-max-grid-quantity-public]

公共接口无须鉴权<br>

可通过该接口获取最大网格数量，最小网格数量总是 2。

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/tradingBot/grid/grid-quantity`

> 请求示例

```shell
GET /api/v5/tradingBot/grid/grid-quantity?instId=BTC-USDT-SWAP&runType=1&algoOrdType=contract_grid&maxPx=70000&minPx=50000&lever=5
```

#### 请求参数

| 参数名         | 类型     | 是否必须 | 描述                                                                                                  |
|:------------|:-------|:-----|:----------------------------------------------------------------------------------------------------|
| instId      | String | 是    | 产品ID，如`BTC-USDT`                                                                        |
| runType   | String    | 是        | 网格类型<br>`1`: 等差<br> `2`: 等比 |
| algoOrdType | String | 是       | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托  |
| maxPx       | String   |是       |  区间最高价格           |
| minPx       | String   |是       |  区间最低价格           |
| lever | String    | 可选         | 杠杆倍数, 合约网格时必填                                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "maxGridQty": "285"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**     | **类型** | **描述** |
|:------------| :------- |:-------|
| maxGridQty      | String   | 最大网格数量 |
