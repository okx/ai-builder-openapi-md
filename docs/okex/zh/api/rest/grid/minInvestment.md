---
site: okex
lang: zh
name: POST / 计算最小投资数量（公共）
method: POST
path: /api/v5/tradingBot/grid/min-investment
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 计算最小投资数量（公共） [order-book-trading-grid-trading-post-compute-min-investment-public]

公共接口无须鉴权

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`POST /api/v5/tradingBot/grid/min-investment`

> 请求示例

```shell
POST /api/v5/tradingBot/grid/min-investment
body
{
    "instId": "ETH-USDT",
    "algoOrdType":"grid",
    "gridNum": "50",
    "maxPx":"5000",
    "minPx":"3000",
    "runType":"1",
    "investmentData":[
        {
            "amt":"0.01",
            "ccy":"ETH"
        },
        {
            "amt":"100",
            "ccy":"USDT"
        }
    ]
}
```

#### 请求参数

| 参数名               | 类型              | 是否必须  | 描述                                                     |
|:------------------|:----------------|:------|:-------------------------------------------------------|
| instId            | String          | 是     | 产品ID，如`BTC-USDT`                                       |
| algoOrdType       | String          | 是     | 策略订单类型<br>`grid`：现货网格委托<br>`contract_grid`：合约网格委托      |
| gridNum           | String          | 是     | 网格数量                                                   |
| maxPx             | String          | 是     | 区间最高价格                                                 |
| minPx             | String          | 是     | 区间最低价格                                                 |
| runType           | String          | 是     | 网格类型<br>`1`：等差，`2`：等比        |
| direction         | String          | 可选    | 合约网格类型<br>`long`：做多，`short`：做空，`neutral`：中性<br>适用于合约网格 |
| lever             | String          | 可选    | 杠杆倍数<br>适用于合约网格                                        |
| basePos           | Boolean         | 否     | 是否开底仓<br>默认为`false`                                    |
| investmentType           | String          | 否       | 投资类型, 仅适用于现货网格<br> `quote`: 计价货币<br>`base`: 交易货币<br>`dual`: 计价货币和交易货币 |
| triggerStrategy           | String          | 否       | 触发策略, <br> `instant`: 立即触发 <br>`price`: 价格触发<br>`rsi`: rsi 触发 |
| topUpAmt   | String | 否       | 增加的投资额，仅适用于现货网格                                                                          |
| investmentData | Array of objects | 否     | 投资信息                                                   |
| > amt             | String         | 是     | 投资数量                                                   |
| > ccy             | String         | 是     | 投资币种                                                   |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
           "minInvestmentData": [  
               {
                   "amt":"0.1",
                   "ccy":"ETH"
               },
               {
                   "amt":"100",
                   "ccy":"USDT"
               }
           ],
           "singleAmt":"10"
       }
    ]
}
```

#### 返回参数

| **参数名**        | **类型**          | **描述**                             |
|:---------------|:----------------|:-----------------------------------|
| minInvestmentData | Array of objects | 最小投入信息                             |
| > amt          | String         | 最小投入数量                             |
| > ccy          | String         | 最小投入币种                             |
| singleAmt      | String          | 单网格买卖量<br>现货网格单位为计价币<br>合约网格单位为张   |
