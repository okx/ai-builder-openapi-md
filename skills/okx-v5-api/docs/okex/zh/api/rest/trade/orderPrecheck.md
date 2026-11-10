---
site: okex
lang: zh
name: POST / 订单预检查
method: POST
path: /api/v5/trade/order-precheck
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 订单预检查 [order-book-trading-trade-post-order-precheck]

用来预先查看订单下单前后的账户的对比信息，仅适用于`跨币种保证金模式`和`组合保证金模式`。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/trade/order-precheck`

> 请求示例

```shell
POST /api/v5/trade/order-precheck
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "clOrdId":"b15",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
}

```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                           |
| :----- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId            | String   | 是         | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                                            |
| tdMode            | String   | 是       | 交易模式<br>保证金模式：`isolated`：逐仓 ；`cross`：全仓 <br>非保证金模式：`cash`：非保证金<br>`spot_isolated`：现货逐仓(仅适用于现货带单) ，现货带单时，`tdMode` 的值需要指定为`spot_isolated`                                                                                                                                      |
| side              | String   | 是       | 订单方向<br>`buy`：买，   `sell`：卖                                                                                                                                                                                                                                                  |
| posSide           | String   | 可选     | 持仓方向<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`。 仅适用交割、永续。                                                                                                                                                                                                                          |
| ordType           | String   | 是       | 订单类型  <br>`market`：市价单<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`optimal_limit_ioc`：市价委托立即成交并取消剩余（仅适用交割、永续）<br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。）                       |
| sz                | String   | 是       | 委托数量                                                                                                                                                                                                                                                                         |
| px                | String   | 可选     | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`类型的订单                                                                                                                                                                       |
| outcome | String | 可选 | 用户交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，且为必填 |
| reduceOnly        | Boolean  | 否       | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                                                                                                                  |
| tgtCcy            | String   | 否       | 市价单委托数量`sz`的单位，仅适用于`币币`市价订单<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>买单默认`quote_ccy`， 卖单默认`base_ccy`                                                                                                                                                                         |
| attachAlgoOrds         | Array of objects   | 否    | 附带止盈止损或移动止盈止损订单信息                                                                                                                                                                                                                                                                   |
| > attachAlgoClOrdId    | String            | 否    | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给`algoClOrdId`                                                                                                                                                                    |
| > tpTriggerPx          | String            | 可选    | 止盈触发价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈委托价                                                                                                                                                                                                                                         |
| > tpOrdPx              | String            | 可选    | 止盈委托价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈触发价<br/>对于限价止盈单，需填写此参数，不需要填写止盈触发价<br/>委托价格为-1时，执行市价止盈                                                                                                                                                                                                                |
| > tpOrdKind | String | 否       | 止盈订单类型<br/>`condition`: 条件单<br/>`limit`: 限价单<br/> 默认为`condition`                                                                                                                                                                                                             |
| > slTriggerPx          | String            | 可选    | 止损触发价，如果填写此参数，必须填写 止损委托价                                                                                                                                                                                                                                                     |
| > slOrdPx              | String            | 可选    | 止损委托价，如果填写此参数，必须填写 止损触发价<br>委托价格为-1时，执行市价止损                                                                                                                                                                                                                                  |
| > tpTriggerPxType      | String            | 否    | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                                           |
| > slTriggerPxType      | String            | 否    | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                                           |
| > sz                   | String            | 可选   | 数量。仅适用于”多笔止盈”的止盈订单，且对于”多笔止盈”的止盈订单必填                                                                                                                                                                                                                                          |
| > callbackRatio       | String | 可选 | 回调幅度的比例，如 `0.05` 代表 5%。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > callbackSpread      | String | 可选 | 回调幅度的价距。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > activePx            | String | 否          | 激活价格。<br>激活价格是移动止盈止损的激活条件，当市场最新成交价达到或超过激活价格，委托被激活。激活后系统开始计算止盈止损的实际触发价格。如果不填写激活价格，即下单后就被激活。<br>仅适用于 `ordType` = `move_order_stop` |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "41.94347460746277",
            "adjEqChg": "-226.05616481626",
            "availBal": "0",
            "availBalChg": "0",
            "imr": "0",
            "imrChg": "57.74709688430927",
            "liab": "0",
            "liabChg": "0",
            "liabChgCcy": "",
            "liqPx": "6764.8556232031115",
            "liqPxDiff": "-57693.044376796888536773622035980224609375",
            "liqPxDiffRatio": "-0.8950500152315991",
            "mgnRatio": "0",
            "mgnRatioChg": "0",
            "mmr": "0",
            "mmrChg": "0",
            "posBal": "",
            "posBalChg": "",
            "type": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**          | **类型** | **描述**                                                                                           |
|:-----------------| :------- |:-------------------------------------------------------------------------------------------------|
| adjEq            | String     | 当前美金层面有效保证金                                                                                      |
| adjEqChg         | String     | 下单后，美金层面有效保证金的变动数量                                                                               |
| imr              | String     | 当前美金层面占用保证金                                                                                      |
| imrChg           | String     | 下单后，美金层面占用保证金的变动数量                                                                               |
| mmr              | String     | 当前美金层面维持保证金                                                                                      |
| mmrChg           | String     | 下单后，美金层面维持保证金的变动数量                                                                               |
| mgnRatio         | String     | 当前美金层面维持保证金率                                                                                       |
| mgnRatioChg      | String     | 下单后，美金层面维持保证金率的变动数量                                                                                |
| availBal         | String     | 当前币种可用余额，仅适用于关闭自动借币时                                                                          |
| availBalChg         | String     | 下单后，币种可用余额的变动数量，仅适用于关闭自动借币时                                                                   |
| liqPx         | String     | 当前预估强平价                                                                                          |
| liqPxDiff         | String     | 下单后，预估强平价与标记价格的差距                                                                                |
| liqPxDiffRatio         | String     | 下单后，预估强平价与标记价格的差距比率                                                                              |
| posBal         | String     | 当前杠杆逐仓仓位正资产，仅适用于逐仓杠杆                                                                             |
| posBalChg         | String     | 下单后，杠杆逐仓仓位正资产的变动数量，仅适用于逐仓杠杆                                                                      |
| liab         | String     | 当前负债<br/> 如果是全仓，对应全仓负债，如果是逐仓，对应逐仓负债                                                              |
| liabChg         | String     | 下单后，当前负债的变动数量<br/> 如果是全仓，对应全仓负债，如果是逐仓，对应逐仓负债                                                     |
| liabChgCcy         | String     | 下单后，当前负债变动数量的单位<br/>仅适用于全仓，开启自动借币时                                                                 |
| type         | String     | 仓位正资产(`posBal`)的单位类型，仅适用于杠杆逐仓，用来确定`posBal`的单位 <br/>`1`:下单前后都是交易货币<br/> `2`:下单前是交易货币，下单后是计价货币<br> `3`:下单前是计价货币，下单后是交易货币<br> `4`:下单前后都是计价货币 |
