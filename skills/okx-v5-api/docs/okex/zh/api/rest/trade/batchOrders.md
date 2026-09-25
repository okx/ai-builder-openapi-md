---
site: okex
lang: zh
name: POST / 批量下单
method: POST
path: /api/v5/trade/batch-orders
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 批量下单 [order-book-trading-trade-post-place-multiple-orders]

每次最多可以批量提交20个新订单。请求参数应该按数组格式传递，会依次委托订单。<br><br>

#### 限速：300个/2s 
#### 跟单交易带单员带单产品的限速：4个/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`下单`限速中。
</aside>

#### HTTP请求

`POST /api/v5/trade/batch-orders`

> 请求示例

```shell
# 币币批量下单
 POST /api/v5/trade/batch-orders
 body
 [
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b15",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    },
    {
        "instId":"BTC-USDT",
        "tdMode":"cash",
        "clOrdId":"b16",
        "side":"buy",
        "ordType":"limit",
        "px":"2.15",
        "sz":"2"
    }
]
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 批量下单
place_orders_without_clOrdId = [
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b15", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"},
    {"instId": "BTC-USDT", "tdMode": "cash", "clOrdId": "b16", "side": "buy", "ordType": "limit", "px": "2.15", "sz": "2"}
]

result = tradeAPI.place_multiple_orders(place_orders_without_clOrdId)
print(result)
```

#### 请求参数

| 参数名                  | 类型              | 是否必须 | 描述                                                                                                                                                                                                                                                      |
|:----------------------|:-----------------|:-------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId               | String           | 是      | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                          |
| tdMode               | String           | 是      | 交易模式<br>保证金模式：`isolated`：逐仓 ；`cross`：全仓 <br>非保证金模式：`cash`：非保证金<br>`spot_isolated`：现货逐仓(仅适用于现货带单) ，现货带单时，`tdMode` 的值需要指定为`spot_isolated`<br>注意：`isolated` 在跨币种保证金模式和组合保证金模式下不可用。<br><br><font color="red">事件合约对应交易产品仅支持`isolated`逐仓下单</font>                                                                                                                                                                                             |
| ccy                  | String           | 条件必填 | 保证金币种<br>通常可选；逐仓杠杆订单及`合约模式`下的全仓杠杆订单必填                                                                                                                                                                                                               |
| clOrdId              | String           | 否      | 客户自定义订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                  |
| tag                  | String           | 否      | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-16位之间。                                                                                                                                                                                                       |
| side                 | String           | 是      | 订单方向 `buy`：买，   `sell`：卖                                                                                                                                                                                                                                |
| posSide              | String           | 可选     | 持仓方向<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`。 仅适用交割、永续。                                                                                                                                                                                                   |
| ordType              | String           | 是      | 订单类型  <br>`market`：市价单，仅适用于`币币/杠杆/交割/永续`<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`optimal_limit_ioc`：市价委托立即成交并取消剩余（仅适用交割、永续）<br>`mmp`：做市商保护(仅适用于组合保证金账户模式下的期权订单)<br>`mmp_and_post_only`：做市商保护且只做maker单(仅适用于组合保证金账户模式下的期权订单)<br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。） |
| sz                   | String           | 是      | 委托数量                                                                                                                                                                                                                                                    |
| px                   | String           | 可选     | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`、`mmp`、`mmp_and_post_only`类型的订单<br>期权下单时，px/pxUsd/pxVol 只能填一个                                                                                                                                                                                                    |
| speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| outcome | String | 可选 | 用户交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，且为必填 |
| pxUsd                | String           | 可选     | 以USD价格进行期权下单 <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个 |
| pxVol                | String           | 可选     | 以隐含波动率进行期权下单，例如 1 代表 100% <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个 |
| reduceOnly           | Boolean          | 否      | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                                                                                             |
| tgtCcy               | String           | 否      | 市价单委托数量`sz`的单位，仅适用于`币币`市价订单<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>买单默认`quote_ccy`， 卖单默认`base_ccy`                                                                                                                                                    |
| banAmend             | Boolean          | 否      | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                                                                                                                                                                |
| pxAmendType          | String           | 否      | 订单价格修正类型<br>`0`：当`px`超出价格限制时，不允许系统修改订单价格 <br>`1`：当`px`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| stpMode              | String           | 否      | 自成交保护模式 <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both不支持FOK<br><br>默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。|
| tradeQuoteCcy        | String           | 否      | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| slippagePct          | String           | 否      | 币币、币币杠杆市价单（`tgtCcy` 为到手币种：买单为 `base_ccy`，卖单为 `quote_ccy`）的最大可接受滑点。<br>取值范围：`0` 至 `0.05`（即 0% 至 5%，含边界），以百分比形式表示时最多保留 2 位小数，例如 `0.01`（1%）和 `0.0123`（1.23%）合法；`0.01234`（1.234%）将被拒绝。<br>不填或为空时，默认为 `0.00%`。<br>不支持改单修改滑点，如需调整请撤单重新提交。<br>仅适用于币币和币币杠杆的市价单。 |
| rpiTakerAccess | Boolean | 否 | 默认值为 `false`。<br>设为 `true` 时，订单可使用 RPI 流动性，适用于 `limit`、`market`、`fok`、`ioc` 订单。<br>当 `rpiTakerAccess` 为 `true` 时，减速带机制在下单和改单时均适用于所有 `ordType`，包括 `post_only`。<br>改单时不会从原始订单继承，必须每次显式指定（省略则该次改单视为 `false`）。<br>`isElpTakerAccess` 在 2026年10月31日前作为别名继续被接受。 |
| rpiPxRound | Boolean | 否 | 默认值为 `false`。仅对 RPI 做市商订单（`ordType: rpi`）生效。设为 `true` 时，违反 RPI 做市商间距规则的价格将自动向外取整至最近的合规价位，而非被拒绝。对非 RPI 订单及 `OPTION`/`EVENTS` 无效。 |
| attachAlgoOrds       | Array of objects | 否      | 附带止盈止损或移动止盈止损订单信息                                                                                                   |
| > attachAlgoClOrdId  | String           | 否      | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给`algoClOrdId` |
| > tpTriggerPx        | String           | 可选     | 止盈触发价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈委托价                                                                                                                                                                                                                                         |
| > tpTriggerRatio    | String          | 可选     | 止盈触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约 <br> `tpTriggerPx` 和 `tpTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须大于 0，如果主单为卖出订单，必须处于 -1 和 0 之间。                                                                           |
| > tpOrdPx            | String           | 可选     | 止盈委托价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈触发价<br/>对于限价止盈单，需填写此参数，不需要填写止盈触发价<br/>委托价格为-1时，执行市价止盈                                                                                                                                                                                                                |
| > tpOrdKind          | String           | 否      | 止盈订单类型<br/>`condition`: 条件单<br/>`limit`: 限价单<br/> 默认为`condition` |
| > slTriggerPx        | String           | 可选     | 止损触发价，如果填写此参数，必须填写 止损委托价                                                                                     |
| > slTriggerRatio    | String          | 可选     | 止损触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约 <br> `slTriggerPx` 和 `slTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须处于 0 和 1 之间，如果主单为卖出订单，必须大于 0。0 代表删除止损。                                                                           |
| > slOrdPx            | String           | 可选     | 止损委托价，如果填写此参数，必须填写 止损触发价<br>委托价格为-1时，执行市价止损                                                                 |
| > tpTriggerPxType    | String           | 否      | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                    |
| > slTriggerPxType    | String           | 否      | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                    |
| > sz                 | String           | 可选     | 数量。仅适用于"多笔止盈"的止盈订单，且对于"多笔止盈"的止盈订单必填                                                                          |
| > amendPxOnTriggerType | String         | 否      | 是否启用开仓价止损，仅适用于分批止盈的止损订单，第一笔止盈触发时，止损触发价格是否移动到开仓均价止损<br>`0`：不开启，默认值 <br>`1`：开启，且止损触发价不能为空              |
| > callbackRatio       | String | 可选 | 回调幅度的比例，如 `0.05` 代表 5%。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > callbackSpread      | String | 可选 | 回调幅度的价距。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > activePx            | String | 否          | 激活价格。<br>激活价格是移动止盈止损的激活条件，当市场最新成交价达到或超过激活价格，委托被激活。激活后系统开始计算止盈止损的实际触发价格。如果不填写激活价格，即下单后就被激活。<br>仅适用于 `ordType` = `move_order_stop` |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        },
        {
            "clOrdId":"oktswap7",
            "ordId":"12344",
            "tag":"",
            "ts":"1695190491421",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                      |
| :--------- | :------- | :---------------------------- |
| code       | String   | 结果代码，`0`表示成功 |
| msg        | String   | 错误信息，代码为0时，该字段为空  |
| data       | Array of objects   | 包含结果的对象数组              |
| > ordId      | String   | 订单ID                        |
| > clOrdId    | String   | 客户自定义订单ID              |
| > tag        | String   | 订单标签                      |
| > ts    | String  | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。  |
| > sCode      | String   | 事件执行结果的code，0代表成功 |
| > sMsg       | String   | 事件执行失败或成功时的msg           |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` <br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">

在组合保证金账户模式下，或者全部成功，或者全部失败。
</aside>

<aside class="notice">
clOrdId<br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有挂单和当前请求中的clOrdId重复。
</aside>
