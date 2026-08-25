---
site: okex
lang: zh
name: GET / 获取马丁策略子订单列表
method: GET
path: /api/v5/tradingBot/dca/orders
module: tradingBot
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取马丁策略子订单列表 [order-book-trading-dca-trading-get-dca-sub-orders]
#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求
`GET /api/v5/tradingBot/dca/orders`

> 请求示例

```shell
GET /api/v5/tradingBot/dca/orders?algoId=2833925189933756416&algoOrdType=contract_dca
```

#### 请求参数
| 参数名      | 类型   | 是否必须 | 描述                                                                          |
| :---------- | :----- | :------- | :---------------------------------------------------------------------------- |
| algoId      | String | 是       | 策略订单 ID                                                                   |
| algoOrdType | String | 是       | 策略订单类型<br>`contract_dca`：合约马丁委托<br>`spot_dca`：现货马丁委托      |
| cycleId     | String | 否       | 策略周期 ID                                                                   |
| after       | String | 否       | 请求此ID之前（更旧的数据）的分页内容，传的值为对应接口的 `ordId`              |
| before      | String | 否       | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的 `ordId`              |
| limit       | String | 否       | 返回结果的数量，最大为100，默认100条                                          |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "cycleId": "9876543",
            "ordId": "570627699870375936",
            "avgFillPx": "41500",
            "direction": "long",
            "side": "buy",
            "ordType": "init_order",
            "px": "41000",
            "sz": "10",
            "filledSz": "10",
            "state": "filled",
            "fee": "-0.2",
            "rebate": "0",
            "rebateCcy": "USDT",
            "lever": "3",
            "instId": "BTC-USDT-SWAP",
            "ctVal": "0.01",
            "fillTime": "1597026383085",
            "cTime": "1597026383085",
            "uTime": "1597026383085",
            "tradeQuoteCcy": ""
        }
    ]
}
```

#### 返回参数
| 参数名        | 类型   | 描述                                                                                                                                                                                                      |
| :------------ | :----- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| cycleId       | String | 策略周期 ID                                                                                                                                                                                               |
| ordId         | String | 子订单 ID                                                                                                                                                                                                 |
| avgFillPx     | String | 子订单平均成交价格                                                                                                                                                                                        |
| direction     | String | 持仓方向<br>合约马丁类型：`long`：多仓，`short`：空仓<br>现货马丁类型：`long`：做多                                                                                                                       |
| side          | String | 子订单方向<br>`buy`：买<br>`sell`：卖                                                                                                                                                                     |
| ordType       | String | 子订单类型<br>`init_order`：初始订单<br>`safety_order`：加仓订单<br>`tp_order`：止盈单<br>`sl_order`：止损单<br>`manual_add_order`：手动加仓单<br>`close_position`：平仓单<br>`manual_close_position`：手动平仓单 |
| px            | String | 子订单委托价格                                                                                                                                                                                            |
| sz            | String | 子订单委托数量                                                                                                                                                                                            |
| filledSz      | String | 子订单成交数量                                                                                                                                                                                            |
| state         | String | 子订单状态<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交<br>`canceled`：撤单成功<br>`cancelling`：撤单中                                                                       |
| fee           | String | 子订单手续费数量                                                                                                                                                                                          |
| rebate        | String | 子订单返佣数量                                                                                                                                                                                            |
| rebateCcy     | String | 子订单返佣币种                                                                                                                                                                                            |
| lever         | String | 杠杆倍数<br>仅适用于 `contract_dca`                                                                                                                                                                       |
| instId        | String | 产品 ID，如 `BTC-USDT-SWAP`                                                                                                                                                                               |
| ctVal         | String | 合约面值<br>仅适用于 `contract_dca`                                                                                                                                                                       |
| fillTime      | String | 子订单成交时间，Unix 时间戳毫秒数，如 `1597026383085`                                                                                                                                                     |
| cTime         | String | 子订单创建时间，Unix 时间戳毫秒数，如 `1597026383085`                                                                                                                                                     |
| uTime         | String | 子订单更新时间，Unix 时间戳毫秒数，如 `1597026383085`                                                                                                                                                     |
| tradeQuoteCcy | String | 指定交易计价货币<br>仅适用于 `spot_dca`                                                                                                                                                                   |
