---
site: okex
lang: zh
name: GET / 获取策略委托单信息
method: GET
path: /api/v5/trade/order-algo
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取策略委托单信息 [order-book-trading-algo-trading-get-algo-order-details]

#### 限速：20次/2s 
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/trade/order-algo`

> 请求示例

```shell
GET /api/v5/trade/order-algo?algoId=1753184812254216192
```

#### 请求参数

| 参数名         | 类型     | 是否必须 | 描述                                                                                                       |
|:------------|:-------|:-----|:---------------------------------------------------------------------------------------------------------|
| algoId      | String | 可选    | 策略委托单ID<br>`algoId`和`algoClOrdId`必须传一个，若传两个，以`algoId`为主                                                            |
| algoClOrdId | String | 可选   | 客户自定义策略订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "activePx": "",
            "actualPx": "",
            "actualSide": "",
            "actualSz": "0",
            "algoClOrdId": "",
            "algoId": "1753184812254216192",
            "amendPxOnTriggerType": "0",
            "attachAlgoOrds": [],
            "cTime": "1724751378980",
            "callbackRatio": "",
            "callbackSpread": "",
            "ccy": "",
            "chaseType": "",
            "chaseVal": "",
            "clOrdId": "",
            "closeFraction": "",
            "failCode": "0",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "isTradeBorrowMode": "",
            "last": "62916.5",
            "lever": "",
            "linkedOrd": {
                "ordId": ""
            },
            "maxChaseType": "",
            "maxChaseVal": "",
            "moveTriggerPx": "",
            "ordId": "",
            "ordIdList": [],
            "ordPx": "",
            "ordType": "conditional",
            "posSide": "net",
            "pxLimit": "",
            "pxSpread": "",
            "pxVar": "",
            "quickMgnType": "",
            "reduceOnly": "false",
            "side": "buy",
            "slOrdPx": "",
            "slTriggerPx": "",
            "slTriggerPxType": "",
            "state": "live",
            "sz": "10",
            "szLimit": "",
            "tag": "",
            "tdMode": "cash",
            "tgtCcy": "quote_ccy",
            "timeInterval": "",
            "tpOrdPx": "-1",
            "tpTriggerPx": "10000",
            "tpTriggerPxType": "last",
            "triggerPx": "",
            "triggerPxType": "",
            "triggerTime": "",
            "tradeQuoteCcy": "USDT",
            "uTime": "1724751378980"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型**          | **描述**                                                                                                                                                       |
|:----------------------|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType              | String          | 产品类型                                                                                                                                                         |
| instId                | String          | 产品ID                                                                                                                                                         |
| ccy                    | String          | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单以及交割、永续和期权合约订单。                                                                                                                                                                                                                                                   |
| ordId                 | String          | 最新一笔订单ID，即将废弃。                                                                                                                                               |
| ordIdList             | Array of strings | 订单ID列表，当止盈止损存在市价拆单时，会有多个。                                                                                                                                    |
| algoId                | String          | 策略委托单ID                                                                                                                                                      |
| clOrdId               | String          | 客户自定义订单ID                                                                                                                                                    |
| sz                    | String          | 委托数量                                                                                                                                                         |
| closeFraction         | String          | 策略委托触发时，平仓的百分比。1 代表100%                                                                                                                                      |
| ordType               | String          | 订单类型                                                                                                                                                         |
| side                  | String          | 订单方向                                                                                                                                                         |
| posSide               | String          | 持仓方向                                                                                                                                                         |
| tdMode                | String          | 交易模式                                                                                                                                                         |
| tgtCcy                | String          | 币币市价单委托数量`sz`的单位<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                                     |
| state                 | String          | 订单状态<br>`live`：待生效<br>`pause`：暂停生效 <br>`partially_effective`:部分生效<br>`effective`：已生效<br>`canceled`：已撤销 <br>`order_failed`：委托失败 <br>`partially_failed`：部分委托失败 |
| lever                 | String          | 杠杆倍数，0.01到125之间的数值，仅适用于 `币币杠杆/交割/永续`                                                                                                                         |
| tpTriggerPx           | String          | 止盈触发价                                                                                                                                                        |
| tpTriggerPxType       | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                        |
| tpOrdPx               | String          | 止盈委托价                                                                                                                                                        |
| slTriggerPx           | String          | 止损触发价                                                                                                                                                        |
| slTriggerPxType       | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                        |
| slOrdPx               | String          | 止损委托价                                                                                                                                                        |
| triggerPx             | String          | 计划委托触发价格                                                                                                                                                     |
| triggerPxType         | String          | 计划委托触发价格类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                     |
| ordPx                 | String          | 计划委托单的委托价格                                                                                                                                                       |
| advanceOrdType        | String        | 计划委托订单类型                                          |
| actualSz              | String          | 实际委托量                                                                                                                                                        |
| actualPx              | String          | 实际委托价                                                                                                                                                        |
| actualSide            | String          | 实际触发方向<br>`tp`：止盈<br>`sl`：止损<br/>仅适用于`单向止盈止损委托`和`双向止盈止损委托`                                                                                                   |
| triggerTime           | String          | 策略委托触发时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                     |
| pxVar                 | String          | 价格比例<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                                  |
| pxSpread              | String          | 价距<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                                    |
| szLimit               | String          | 单笔数量<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                                  |
| pxLimit               | String          | 挂单限制价<br>仅适用于`冰山委托`和`时间加权委托`                                                                                                                                 |
| tag                   | String          | 订单标签                                                                                                                                                         |
| timeInterval          | String          | 下单间隔<br>仅适用于`时间加权委托`                                                                                                                                         |
| callbackRatio         | String          | 回调幅度的比例<br>仅适用于`移动止盈止损`                                                                                                                                      |
| callbackSpread        | String          | 回调幅度的价距<br>仅适用于`移动止盈止损`                                                                                                                                      |
| activePx              | String          | 移动止盈止损激活价格<br>仅适用于`移动止盈止损`                                                                                                                                   |
| moveTriggerPx         | String          | 移动止盈止损触发价格<br>仅适用于`移动止盈止损`                                                                                                                                   |
| reduceOnly            | String          | 是否只减仓<br>`true`或`false`                                                                                                                                      |
| quickMgnType          | String          | ~~一键借币类型，仅适用于杠杆逐仓的一键借币模式<br>`manual`：手动，`auto_borrow`：自动借币，`auto_repay`：自动还币~~（已弃用）                                                                         |
| last                  | String          | 下单时的最新成交价                                                                                                                                                    |
| failCode              | String          | 代表策略触发失败的原因，已撤销和已生效时为""，委托失败时有值，如 51008；<br>仅适用于单向止盈止损委托、双向止盈止损委托、移动止盈止损委托、计划委托。                                                                             |
| algoClOrdId           | String          | 客户自定义策略订单ID                                                                                                                                                  |
| amendPxOnTriggerType  | String          | 是否启用开仓价止损<br>仅适用于分批止盈的止损订单<br>`0`：不开启，默认值 <br>`1`：开启                                                                                                         |
| attachAlgoOrds        | Array of objects | 附带止盈止损或移动止盈止损订单信息<br>适用于`合约模式/跨币种保证金模式/组合保证金模式`                                                                                                                    |
| > attachAlgoClOrdId   | String          | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给algoClOrdId。                                                         |
| > tpTriggerPx         | String          | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                                                                                    |
| > tpTriggerRatio      | String          | 止盈触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约                                                                            |
| > tpTriggerPxType     | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                        |
| > tpOrdPx             | String          | 止盈委托价，如果填写此参数，必须填写`止盈触发价`<br>委托价格为`-1`时，执行市价止盈                                                                                                               |
| > slTriggerPx         | String          | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                                                                                    |
| > slTriggerRatio      | String          | 止损触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约                                                                            |
| > slTriggerPxType     | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                        |
| > slOrdPx             | String          | 止损委托价，如果填写此参数，必须填写`止损触发价`<br>委托价格为`-1`时，执行市价止损                                                                                                               |
| > callbackRatio       | String | 回调幅度的比例，如 `0.05` 代表 5% |
| > callbackSpread      | String | 回调幅度的价距 |
| > activePx            | String | 激活价格 |
| linkedOrd            | Object          | 止盈订单信息，仅适用于止损单，且该止损订单来自包含限价止盈单的双向止盈止损订单                                                                                      |
| > ordId   | String          | 订单 ID                                                                                                                                                        |
| cTime                 | String          | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                       |
| uTime     | String          | 订单更新时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                                         |
| isTradeBorrowMode            | String          | 是否自动借币<br> true：自动借币<br> false：不自动借币<br>仅适用于计划委托、移动止盈止损和 时间加权策略                                                                                  |
| chaseType | String          | 追逐类型。仅适用于`追逐限价委托`。 |
| chaseVal | String          | 追逐值。仅适用于`追逐限价委托`。               |
| maxChaseType | String          | 最大追逐值的类型。仅适用于`追逐限价委托`。          |
| maxChaseVal | String          | 最大追逐值。仅适用于`追逐限价委托`。             |
| tradeQuoteCcy | String	 | 用于交易的计价币种。 |
