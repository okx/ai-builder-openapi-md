---
site: okex
lang: zh
name: POST / 下单
method: POST
path: /api/v5/trade/order
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 下单 [order-book-trading-trade-post-place-order]

只有当您的账户有足够的资金才能下单。<br><br>

#### 限速：60次/2s
#### 跟单交易带单员带单产品的限速：4次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

#### HTTP请求

`POST /api/v5/trade/order`

> 请求示例

```shell
# 币币下单
POST /api/v5/trade/order
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

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 现货模式限价单
result = tradeAPI.place_order(
    instId="BTC-USDT",
    tdMode="cash",
    clOrdId="b15",
    side="buy",
    ordType="limit",
    px="2.15",
    sz="2"
)

print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                      |
| :----- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId            | String   | 是         | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                                       |
| tdMode            | String   | 是       | 交易模式<br>保证金模式：`isolated`：逐仓（仅限于现货杠杆逐仓）；`cross`：全仓 <br>非保证金模式：`cash`：非保证金<br>`spot_isolated`：现货逐仓(仅适用于现货带单) ，现货带单时，`tdMode` 的值需要指定为`spot_isolated`<br>注意：`isolated`（现货杠杆逐仓）在跨币种保证金模式和组合保证金模式下不可用。<br><br><font color="red">事件合约对应交易产品仅支持`isolated`逐仓下单</font>                                                                                                                                 |
| ccy               | String   | 否       | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                                                                                                                                                                        |
| clOrdId           | String   | 否       | 客户自定义订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                   |
| tag               | String   | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                                                                                                                                                                                         |
| side              | String   | 是       | 订单方向<br>`buy`：买，   `sell`：卖                                                                                                                                                                                                                                             |
| posSide           | String   | 可选     | 持仓方向<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`。 仅适用交割、永续。`SPOT` 或 `MARGIN` 订单请勿传此字段。交割/永续在开平仓模式下如未填写，返回错误码 51000。                                                                                                                                                                                                                     |
| ordType           | String   | 是       | 订单类型  <br>`market`：市价单，仅适用于`币币/杠杆/交割/永续`<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`optimal_limit_ioc`：以价格限制区间的最高买价（买单）或最低卖价（卖单）挂限价单，未成交部分立即取消（IOC）。仅适用交割、永续合约，订单不会以超出当前价格限制边界的价格成交<br>`mmp`：做市商保护(仅适用于组合保证金账户模式下的期权订单)<br>`mmp_and_post_only`：做市商保护且只做maker单(仅适用于组合保证金账户模式下的期权订单)<br>`elp`：流动性增强计划订单 |
| sz                | String   | 是       | 委托数量                                                                                                                                                                                                                                                                    |
| px                | String   | 可选     | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`、`mmp`、`mmp_and_post_only`类型的订单<br>期权下单时，px/pxUsd/pxVol 只能填一个                                                                                                                                                                   |
| speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| outcome | String | 可选 | 用户交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，且为必填 |
| pxUsd | String | 可选 | 以USD价格进行期权下单 <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                                            |
| pxVol | String | 可选 | 以隐含波动率进行期权下单，例如 1 代表 100% <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                               |
| reduceOnly        | Boolean  | 否       | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>适用于`合约模式`/`跨币种保证金模式`                                                                                                                                                                               |
| tgtCcy            | String   | 否       | 市价单委托数量`sz`的单位，仅适用于`币币`市价订单<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>买单默认`quote_ccy`， 卖单默认`base_ccy`                                                                                                                                                                    |
| banAmend   | Boolean | 否    | 是否禁止系统在余额不足时自动缩减币币市价单数量。true 或 false，默认false。<br>为true时：余额不足时，整笔订单将被拒绝。为false（默认）时：系统将缩减 sz 至可用余额所能支持的数量后执行。仅适用于币币市价单                                                                                                                                                                                                 |
| pxAmendType            | String  | 否          | 订单价格修正类型<br>`0`：当`px`超出价格限制时，不允许系统修改订单价格 <br>`1`：当`px`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| slippagePct      | String | 否       | 币币、币币杠杆市价单（`tgtCcy` 为到手币种：买单为 `base_ccy`，卖单为 `quote_ccy`）的最大可接受滑点。<br>取值范围：`0` 至 `0.05`（即 0% 至 5%，含边界），以百分比形式表示时最多保留 2 位小数，例如 `0.01`（1%）和 `0.0123`（1.23%）合法；`0.01234`（1.234%）将被拒绝。<br>不填或为空时，默认为 `0.00%`。<br>不支持改单修改滑点，如需调整请撤单重新提交。<br>仅适用于币币和币币杠杆的市价单。 |
| stpMode | String | 否 | 自成交保护模式 <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both不支持FOK <br><br>默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。|
| isElpTakerAccess | Boolean | 否 | 是否作为 taker 吃单 ELP<br>`true`：该请求能吃单 ELP，但会被施加延迟<br>`false`：该请求不能吃单 ELP，并且没有延迟<br><br>默认值为`false`，`true`仅适用于ioc订单 |
| attachAlgoOrds         | Array of objects   | 否    | 附带止盈止损或移动止盈止损订单信息                                                                                                                                                                                                                                                              |
| > attachAlgoClOrdId    | String            | 否    | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给`algoClOrdId`                                                                                                                                                               |
| > tpTriggerPx          | String            | 可选    | 止盈触发价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈委托价                                                                                                                                                                                                                                    |
| > tpTriggerRatio    | String          | 可选     | 止盈触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约<br> `tpTriggerPx` 和 `tpTriggerRatio` 只能传入其中一个<br>如果主单为买入订单，必须大于 0，如果主单为卖出订单，必须处于 -1 和 0 之间。                                                                            |
| > tpOrdPx              | String            | 可选    | 止盈委托价<br/>对于条件止盈单，如果填写此参数，必须填写 止盈触发价<br/>对于限价止盈单，需填写此参数，不需要填写止盈触发价<br/>委托价格为-1时，执行市价止盈                                                                                                                                                                                  |
| > tpOrdKind | String | 否       | 止盈订单类型<br/>`condition`: 条件单<br/>`limit`: 限价单<br/> 默认为`condition`                                                                                                                                                                                                        |
| > slTriggerPx          | String            | 可选    | 止损触发价，如果填写此参数，必须填写 止损委托价                                                                                                                                                                                                                                                |
| > slTriggerRatio    | String          | 可选     | 止损触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约<br> `slTriggerPx` 和 `slTriggerRatio` 只能传入其中一个<br>如果主单为买入订单，必须处于 0 和 1 之间，如果主单为卖出订单，必须大于 0。                                                                            |
| > slOrdPx              | String            | 可选    | 止损委托价，如果填写此参数，必须填写 止损触发价<br>委托价格为-1时，执行市价止损                                                                                                                                                                                                                             |
| > tpTriggerPxType      | String            | 否    | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                                      |
| > slTriggerPxType      | String            | 否    | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                                      |
| > sz                   | String            | 可选   | 数量。仅适用于“多笔止盈”的止盈订单，且对于“多笔止盈”的止盈订单必填                                                                                                                                                                                                                                     |
| > amendPxOnTriggerType | String            | 否    | 是否启用开仓价止损，仅适用于分批止盈的止损订单，第一笔止盈触发时，止损触发价格是否移动到开仓均价止损<br>`0`：不开启，默认值 <br>`1`：开启，且止损触发价不能为空                                                                                                                                                                                 |
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
            "subCode": ""
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
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（事件执行失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` <br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">
tdMode<br>
交易模式，下单时需要指定<br>
<strong>现货模式：</strong><br>
- 币币和期权买方：cash<br>
<strong>合约模式：</strong><br>
- 逐仓杠杆（仅限于现货杠杆逐仓）：isolated<br>
- 全仓杠杆：cross<br>
- 币币：cash<br>
- 全仓交割/永续/期权：cross<br>
<strong>跨币种保证金模式：</strong><br>
- 全仓币币：cross<br>
- 全仓交割/永续/期权：cross<br>
<strong>组合保证金模式：</strong><br>
- 全仓币币：cross<br>
- 全仓交割/永续/期权：cross<br>
</aside>

<aside class="notice">
clOrdId<br>
clOrdId 是用户在 User ID 维度自定义的订单唯一标识符。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。<br>
clOrdId 不能与当前所有挂单（live 或 partially_filled 状态）的 clOrdId 重复。订单达到终态（filled、canceled、mmp_canceled）后，相同的 clOrdId 可重新用于新订单。系统不强制历史唯一性——当多笔订单共享同一 clOrdId 时，GET /api/v5/trade/order 仅返回最新一笔。"普通委托单"指通过本接口下的标准订单；clOrdId 不会传递至附带的止盈止损策略订单。
</aside>

<aside class="notice">
posSide<br>
持仓方向，买卖模式下此参数非必填，如果填写仅可以选择net；在开平仓模式下必填，且仅可选择 long 或 short。<br>
开平仓模式下，side和posSide需要进行组合<br>
开多：买入开多（side 填写 buy；  posSide 填写 long ）<br>
开空：卖出开空（side 填写 sell； posSide 填写 short ）<br>
平多：卖出平多（side 填写 sell；posSide 填写 long ）<br>
平空：买入平空（side 填写 buy； posSide 填写 short ）<br>
组合保证金模式：交割和永续仅支持买卖模式<br>
SPOT 或 MARGIN 订单请勿传此字段。交割/永续在买卖模式下可不传或传 `net`。
</aside>

<aside class="notice">
ordType<br>
订单类型，创建新订单时必须指定，您指定的订单类型将影响需要哪些订单参数和撮合系统如何执行您的订单，以下是有效的ordType：<br>
普通委托：<br>
limit：限价单，要求指定sz 和 px<br>
market：市价单，币币和币币杠杆，是市价委托吃单；交割合约和永续合约，是自动以最高买/最低卖价格委托，遵循限价机制；期权合约不支持市价委托；由于市价委托无法确定成交价格，为确保有足够的资产买入设定数量的交易币种，会多冻结5%的计价币资产<br>
高级委托：<br>
post_only：限价委托，在下单那一刻只做maker，如果该笔订单的任何部分会吃掉当前挂单深度，则该订单将被全部撤销。<br>
fok：限价委托，全部成交或立即取消，如果无法全部成交该笔订单，则该订单将被全部撤销。<br>
ioc：限价委托，立即成交并取消剩余，立即按照委托价格撮合成交，并取消该订单剩余未完成数量，不会在深度列表上展示委托数量。<br>
optimal_limit_ioc：以价格限制区间的最高买价（买单）或最低卖价（卖单）挂限价单，未成交部分立即取消（IOC），仅适用于交割合约和永续合约。订单不会以超出当前价格限制边界的价格成交。
</aside>

<aside class="notice">
sz<br>
交易数量，表示要购买或者出售的数量。<br>
当币币/币币杠杆以限价买入和卖出时，指交易货币数量。<br>
当币币杠杆以市价买入时，指计价货币的数量。<br>
当币币杠杆以市价卖出时，指交易货币的数量。<br>
对于币币市价单，单位由 tgtCcy 决定<br>
当交割、永续、期权买入和卖出时，指合约张数。合约面值 = sz × ctVal × markPx（正向合约）或 sz × ctVal（反向合约，USD 计价）。ctVal 和 ctType 可通过 GET /api/v5/public/instruments 获取。
</aside>

<aside class="notice">
reduceOnly<br>
只减仓，下单时，此参数设置为 true 时，表示此笔订单具有减仓属性，只会减少持仓数量，不会增加新的持仓仓位<br>
对于同一杠杆产品，所有反方向挂单的币数加上当前只减仓下单数量，不能超过仓位资产；负债还完后，如果还有剩余的委托数量，不会反向开仓，而是会进行币币交易。<br>
对于同一交割/永续产品，当前只减仓下单张数，加上价格时间优先于当前只减仓下单的只减仓挂单张数总和，不能超过持仓数量<br>
仅适用于`合约模式`和`跨币种保证金模式`<br>
仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>
注意：交割和永续合约在开平仓模式下，所有的平仓单都有只减仓逻辑，不受该字段传值的影响。<br>
如果 sz 超过当前持仓数量，整笔订单同样会被拒绝——系统不会自动截取至持仓数量。
</aside>

<aside class="notice">
tgtCcy<br>
市价单委托数量`sz`的单位：仅适用于币币市价下单交易。<br>
快速参考（以 BTC-USDT 为例）：<br>
- tgtCcy=`quote_ccy`，sz=100（买入）：花费 100 USDT 购买 BTC。<br>
- tgtCcy=`base_ccy`，sz=0.001（买入）：以市价买入 0.001 BTC。<br>
- tgtCcy=`base_ccy`，sz=0.001（卖出，默认）：卖出 0.001 BTC。<br>
- tgtCcy=`quote_ccy`，sz=100（卖出）：卖出 BTC 直至收到 100 USDT。<br>
交易货币：base_ccy<br>
计价货币：quote_ccy

<br>

您在使用交易货币买入或者计价货币卖出时，请知晓：
<br>
1.如果您输入的数量大于当前可买或者可卖的数量，系统将按照您的最大可买或者可卖数量帮您完成交易，如果您希望按照指定数量成交，那您可以尝试使用限价单，等待市场价格波动到锁定的余额可以买入或卖出您指定的数量。
<br>
2.如果您输入的数量不大于当前可买或者可卖的数量，那当市场价格波动过大时，锁定的余额可能没办法买入您输入的交易货币数量或卖出您输入的计价货币数量，为保证您的交易体验，我们基于【能买多少买多少】或者【能卖多少卖多少】的原则，更改下单的数量帮您完成交易。此外，我们将尽量多锁定一点余额来规避更改下单数量的情况。
<br>
2.1 交易币买入例子：
<br>
以市价下单 买入 10个LTC为例，用户可买为11个，此时 10 < 11，挂单成功。当LTC-USDT的市价为200，用户被锁定余额为3,000 USDT，200*10 < 3,000，最终成交10个LTC；
若市场波动过大，LTC-USDT的市价为400，此时400*10 > 3,000，当用户被锁定的余额不够买入下单指定的交易货币数量时，系統使用用户被锁定的最大余额3,000 USDT下单买入，最终成交 3,000/400 = 7.5个 LTC。
<br>
2.2 计价币卖出例子：
<br>
以市价下单 卖出 1,000USDT为例，用户可卖为1,200USDT，1,000 < 1,200，挂单成功。LTC-USDT的市价为200，用户被锁定的余额为6个LTC，最终成交5个LTC；
若市场波动过大，LTC-USDT的市价为100，100*6 < 1,000，当用户被锁定的余额不够卖出下单指定的计价货币数量时，系統使用用户被锁定的最大余额6个LTC下单，最终成交 6 * 100 = 600 USDT。
</aside>

<aside class="notice">
px<br>
期权下单时，委托价格需为 tickSz 的整数倍。<br>
当不为整数倍时，取值规则以tickSz取 0.0005 为例：<br>
当委托价格对0.0005的余数大于0.00025或者委托价格小于0.0005时，向上取；<br>
当委托价格对0.0005的余数小于等于0.00025，且委托价格大于0.0005时，向下取。
</aside>

<aside class="notice">
对于下单附带止盈止损：<br>
附带的止盈止损订单仅在母单成交后才会激活。若母单在任何成交前被撤销，附带的止盈止损也将一并丢弃。如需独立于母单的止盈止损，请使用 POST /api/v5/trade/order-algo。<br>
1. 只有当该订单成交时，才会生成止盈止损策略订单；若母单在成交前被撤销，则不会生成止盈止损策略订单。<br>
<br>
2. tgtCcy 为 base_ccy 时的市价买单和 tgtCcy 为 quote_ccy 时的市价卖单，均不支持附带止盈止损<br>
3. tpOrdKind 为 limit，且只有一笔单边止盈时，attachAlgoClOrdId 可以作为 clOrdId 在获取订单信息接口查询。<br>
4. 对于“分批止盈”，包含限价止盈和触发止盈：<br>
* 分批止盈的每笔止盈止损订单仅支持单向止盈止损，slTriggerPx&slOrdPx 与 tpTriggerPx&tpOrdPx 只能填写一组，否则 报错 51076<br>
* 同一笔订单上附带分批止盈的止盈触发价类型 (tpTriggerPxType) 必须保持一致，否则报错 51080<br>
* 同一笔订单上附带分批止盈的止盈触发价 (tpTriggerPx) 不能相等，否则报错 51081<br>
* 在附带分批止盈时，止盈订单的数量不能为空，否则报错 51089<br>
* 同一笔订单上分批止盈的止盈数量之和，需要等于订单的委托数量，否则报错 51083<br>
* 同一笔订单上分批止盈的止盈委托不能超过 10 笔，否则报错 51079<br>
* 币币/杠杆不支持开启'开仓价止损'，否则报错 51077<br>
* 同一笔订单上附带分批止盈的止损委托单不能超过 1 笔，否则报错 51084<br>
* 附带止盈止损开启'开仓价止损'时 (amendPxOnTriggerType 设置为 1)，该笔订单上的止盈委托单必须大于等于 2 笔，否则报错 51085<br>
* 同一笔订单上附带分批止盈的止盈类型必须保持一致，否则报错 51091<br>
* 同一笔订单上附带分批止盈的止盈委托价不能相等，否则报错 51092<br>
* 同一笔订单上附带分批止盈，其中限价止盈的止盈委托价 (tpOrdPx) 不能为 -1 (市价)，否则报错 51093<br>
* 币币、杠杆和期权交易不支持限价止盈，否则报错 51094<br>
</aside>

<aside class="notice">
强制自成交保护<br>
交易系统会以母账户维度实施强制自成交保护，同一母账户下所有账户，包括母账户本身和所有子账户，都无法进行自成交。默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。<br>
强制自成交保护不会导致延迟。<br>
有三种STP模式。STP模式始终基于taker订单中的配置。<br>
1.Cancel Maker：这是默认的STP模式，系统撤Maker订单以防止自成交。然后，taker订单会基于深度继续和下一个订单成交。<br>
2.Cancel Taker：撤Taker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后撤单。FOK订单会确保完全成交和自成交保护。<br>
3.Cancel Both：撤Taker和Maker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后Taker订单的剩余数量和第一个自我Maker订单被取消。此模式不支持FOK订单。将 stpMode=cancel_both 与 ordType=`fok` 组合使用将返回错误码 50016。<br>
</aside>

<aside class="notice">
tradeQuoteCcy<br>
对于特定国家和地区的用户，下单成功需要填写该参数，否则会取 `instId` 的计价币种为默认值，报错 51000。<br>
传值必须取 tradeQuoteCcyList 的枚举值，tradeQuoteCcyList 来自获取交易产品基础信息(GET /api/v5/account/instruments) 接口。<br>
</aside>

<aside class="notice">
isElpTakerAccess:true订单限速<br>
- 50个/2s，限制维度为 User ID + Instrument ID<br>
- 该限速会在 REST 和 WebSocket 的下单及批量下单接口中共享
</aside>
