---
site: okex
lang: zh
name: GET / 网格策略智能回测（公共）
method: GET
path: /api/v5/tradingBot/grid/ai-param
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 网格策略智能回测（公共） [order-book-trading-grid-trading-get-grid-ai-parameter-public]

公共接口无须鉴权

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/tradingBot/grid/ai-param`

> 请求示例

```shell
GET /api/v5/tradingBot/grid/ai-param?instId=BTC-USDT&algoOrdType=grid
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                                 |
| :-------- | :----- | :------- |:-----------------------------------------------------------------------------------|
| algoOrdType | String | 是       | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托                                  |
| instId    | String | 是       | 产品ID，如`BTC-USDT`                                                                   |
| direction | String | 可选   | 合约网格类型<br>`long`：做多，`short`：做空，`neutral`：中性<br>合约网格必填                              |
| duration | String | 否 | 回测时长，单位为天<br>现货网格默认 `7D`，可选：`7D`、`30D`、`180D`<br>合约网格默认 `14D`，可选：`7D`、`14D`、`30D` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoOrdType": "grid",
            "annualizedRate": "1.5849",
            "ccy": "USDT",
            "direction": "",
            "duration": "7D",
            "gridNum": "5",
            "instId": "BTC-USDT",
            "lever": "0",
            "maxPx": "21373.3",
            "minInvestment": "0.89557758",
            "minPx": "15544.2",
            "perGridProfitRatio": "4.566226200302574",
            "perMaxProfitRate": "0.0733865364573281",
            "perMinProfitRate": "0.0561101403446263",
            "runType": "1",
            "sourceCcy": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                            |
| :--------- | :------- |:--------------------------------------------------|
| instId       | String   | 产品ID                                              |
| algoOrdType       | String   | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托 |
| duration       | String   | 回测周期<br>`7D`：7天，`30D`：30天，`180D`：180天             |
| gridNum       | String   | 网格数量                                              |
| maxPx       | String   | 区间最高价格                                            |
| minPx       | String   | 区间最低价格                                            |
| perMaxProfitRate       | String   | 单网格最高利润率                                          |
| perMinProfitRate       | String   | 单网格最低利润率                                          |
| perGridProfitRatio       | String   | 单网格利润率                                            |
| annualizedRate       | String   | 网格年化收益率                                           |
| minInvestment       | String   | 最小投资数量                                            |
| ccy       | String   | 投资币种                                              |
| runType       | String   | 网格类型<br>`1`：等差，`2`：等比                             |
| direction       | String   | 合约网格类型<br>仅适用于`合约网格`                              |
| lever       | String   | 杠杆倍数<br>仅适用于`合约网格`                                |
| sourceCcy       | String   | 来源币种                                              |
