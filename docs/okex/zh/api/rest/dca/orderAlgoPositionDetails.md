---
site: okex
lang: zh
name: GET / 获取马丁策略委托持仓
method: GET
path: /api/v5/tradingBot/dca/position-details
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取马丁策略委托持仓 [order-book-trading-dca-trading-get-dca-position-details]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`GET /api/v5/tradingBot/dca/position-details`

> 请求示例

```shell
GET /api/v5/tradingBot/dca/position-details?algoId=2833925189933756416&algoOrdType=contract_dca
```

#### 请求参数
| 参数名      | 类型   | 是否必须 | 描述                                                                   |
| :---------- | :----- | :------- | :--------------------------------------------------------------------- |
| algoId      | String | 是       | 策略订单 ID                                                             |
| algoOrdType | String | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "algoId": "2833925189933756416",
            "algoClOrdId": "",
            "algoOrdType": "contract_dca",
            "instId": "BTC-USDT-SWAP",
            "curCycleld": "3",
            "startTime": "1597026383085",
            "fillManualOrds": "0",
            "fillSafetyOrds": "2",
            "fundingFee": "-0.05",
            "initPx": "43200",
            "notionalUsd": "5000",
            "avgPx": "43000",
            "upl": "12.5",
            "liqPx": "38000",
            "sz": "2",
            "baseSz": "",
            "quoteSz": "",
            "slPx": "40000",
            "tpPx": "45000",
            "fee": "-0.2",
            "tradeQuoteCcy": ""
        }
    ]
}
```

#### 返回参数
| 参数名         | 类型   | 描述                                                                        |
| :------------- | :----- | :-------------------------------------------------------------------------- |
| algoId         | String | 策略订单 ID                                                                  |
| algoClOrdId    | String | 客户端自定义策略单ID                                                         |
| algoOrdType    | String | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托     |
| instId         | String | 产品ID，如 `BTC-USDT`                                                        |
| curCycleld     | String | 正在运行中的周期 ID                                                          |
| startTime      | String | 当轮周期开启时间，Unix 时间戳的毫秒数格式，如 `1597026383085`                |
| fillManualOrds | String | 周期手动加仓次数                                                             |
| fillSafetyOrds | String | 周期已加仓次数                                                               |
| fundingFee     | String | 当轮周期累计资金费用<br>仅适用于 `contract_dca`                              |
| initPx         | String | 初始订单开仓均价或初始订单成交价                                             |
| notionalUsd    | String | 仓位美金价值<br>仅适用于 `contract_dca`                                      |
| avgPx          | String | 开仓均价                                                                     |
| upl            | String | 未实现收益                                                                   |
| liqPx          | String | 预估强平价<br>仅适用于 `contract_dca`                                        |
| sz             | String | 合约数量<br>仅适用于 `contract_dca`                                          |
| baseSz         | String | 当前周期持有的交易币数量<br>仅适用于 `spot_dca`                              |
| quoteSz        | String | 当前周期持有的计价币数量<br>仅适用于 `spot_dca`                              |
| slPx           | String | 止损价格                                                                     |
| tpPx           | String | 止盈价格                                                                     |
| fee            | String | 累计手续费金额，正数代表平台返佣，负数代表平台扣除                           |
| tradeQuoteCcy  | String | 指定交易计价货币<br>仅适用于 `spot_dca`                                      |
