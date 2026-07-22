---
site: okex
lang: zh
name: WS / 下单
type: guide
---

### WS / 下单 [order-book-trading-trade-ws-place-order]

只有当您的账户有足够的资金才能下单。一旦下单，您的账户资金将在订单生命周期内被冻结。被冻结的资金以及数量取决于订单指定的类型和参数<br><br>

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：60次/2s 
#### 跟单交易带单员带单产品的限速：4次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

<aside class="notice">
同`下单` REST API 共享限速
</aside>

> 请求示例

```shell
{
	"id": "1512",
	"op": "order",
	"args": [{
		"side": "buy",
		"instIdCode": 123456,
		"tdMode": "isolated",
		"ordType": "market",
		"sz": "100"
	}]
}
```

#### 请求参数

| 参数名            | 类型               | 是否必须 | 描述                                                                                                                                                                                                                                                                      |
|:---------------|:-----------------| :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String           | 是       | 消息的唯一标识<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。                                                                                                                                                                                        |
| op             | String           | 是       | 操作<br>`order`                                                                                                                                                                                                                                                           |
| args           | Array of objects | 是       | 请求参数                                                                                                                                                                                                                                                                    |
| > instIdCode   | Integer          | 是       | 产品唯一标识代码。   |
| > tdMode       | String           | 是       | 交易模式<br>保证金模式 `isolated`：逐仓  `cross`：全仓    <br>非保证金模式  `cash`：现金<br>`spot_isolated`：现货逐仓(仅适用于现货带单) ，现货带单时，`tdMode` 的值需要指定为`spot_isolated`  <br><br><font color="red">事件合约对应交易产品仅支持`isolated`逐仓下单</font>                                                                                                                                 |
| > ccy          | String           | 否       | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                                                                                                                                                                     |
| > clOrdId      | String           | 否       | 由用户设置的订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                   |
| > tag          | String           | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-16位之间。                                                                                                                                                                                                                         |
| > side         | String           | 是       | 订单方向，`buy`   `sell`                                                                                                                                                                                                                                                     |
| > posSide      | String           | 否       | 持仓方向<br>在买卖模式下，默认 `net`<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`，仅适用于`交割/永续`                                                                                                                                                                                                 |
| > ordType      | String           | 是       | 订单类型  <br>`market`：市价单，仅适用于`币币/杠杆/交割/永续`<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`optimal_limit_ioc`：市价委托立即成交并取消剩余（仅适用交割、永续）<br>`mmp`：做市商保护(仅适用于组合保证金账户模式下的期权订单)<br>`mmp_and_post_only`：做市商保护且只做maker单(仅适用于组合保证金账户模式下的期权订单)<br>`elp`：流动性增强计划订单 |
| > sz           | String           | 是       | 委托数量                                                                                                                                                                                                                                                                    |
| > px           | String           | 可选     | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`、`mmp`、`mmp_and_post_only`类型的订单<br>期权下单时，px/pxUsd/pxVol 只能填一个                                                                                                                                                                   |
| > speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| > outcome | String | 可选 | 用户交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，且为必填 |
| > pxUsd        | String           | 可选 | 以USD价格进行期权下单 <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                                            |
| > pxVol        | String           | 可选 | 以隐含波动率进行期权下单，例如 1 代表 100% <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                               |
| > reduceOnly   | Boolean          | 否       | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                                                                                                              |
| > tgtCcy       | String           | 否          | 币币市价单委托数量`sz`的单位<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                                                                                                                                                |
| > banAmend     | Boolean          | 否    | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                                                                                                                                                                                 |
| > pxAmendType  | String           | 否       | 订单价格修正类型<br>`0`：当`px`超出价格限制时，不允许系统修改订单价格<br>`1`：当`px`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| > tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| > slippagePct      | String | 否       | 币币、币币杠杆市价单（`tgtCcy` 为到手币种：买单为 `base_ccy`，卖单为 `quote_ccy`）的最大可接受滑点。<br>取值范围：`0` 至 `0.05`（即 0% 至 5%，含边界），以百分比形式表示时最多保留 2 位小数，例如 `0.01`（1%）和 `0.0123`（1.23%）合法；`0.01234`（1.234%）将被拒绝。<br>不填或为空时，默认为 `0.00%`。<br>不支持改单修改滑点，如需调整请撤单重新提交。<br>仅适用于币币和币币杠杆的市价单。 |
| > stpMode      | String           | 否 | 自成交保护模式 <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both不支持FOK <br><br>默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。|
| > isElpTakerAccess | Boolean | 否 | 是否作为 taker 吃单 ELP<br>`true`：该请求能吃单 ELP，但会被施加延迟<br>`false`：该请求不能吃单 ELP，并且没有延迟<br><br>默认值为`false`，`true`仅适用于ioc订单 |
| expTime        | String           | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                |

> 成功返回示例

```json
{
	"id": "1512",
	"op": "order",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}],
	"code": "0",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 失败返回示例

```json
{
	"id": "1512",
	"op": "order",
	"data": [{
		"clOrdId": "",
		"ordId": "",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "51008",
      	"sMsg": "Order failed. Insufficient USDT balance in account",
      	"subCode": "1000"
	}],
	"code": "1",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 格式错误返回示例

```json
{
	"id": "1512",
	"op": "order",
	"data": [],
	"code": "60013",
	"msg": "Invalid args",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| 参数名       | 类型                  | 描述                                                       |
|:----------|:--------------------|:---------------------------------------------------------|
| id        | String              | 消息的唯一标识                                                  |
| op        | String              | 操作<br>`order`                                            |
| code      | String              | 代码                                                       |
| msg       | String              | 消息                                                       |
| data      | Array of objects    | 请求成功后返回的数据                                               |
| > ordId   | String              | 订单ID                                                     |
| > clOrdId | String              | 由用户设置的订单ID                                               |
| > tag     | String              | 订单标签                                                     |
| > ts      | String              | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。           |
| > sCode   | String              | 订单状态码，0 代表成功                                             |
| > sMsg    | String              | 订单状态消息                                                   |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime    | String              | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` |
| outTime   | String              | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123` |

<aside class="notice">
tdMode<br>
交易模式，下单时需要指定<br>
<strong>现货模式：</strong><br>
- 币币和期权买方：cash<br>
<strong>合约模式：</strong><br>
- 逐仓杠杆：isolated<br>
- 全仓杠杆：cross<br>
- 币币：cash<br>
- 全仓交割/永续/期权：cross<br>
- 逐仓交割/永续/期权：isolated<br>
<strong>跨币种保证金模式：</strong><br>
- 逐仓杠杆：isolated<br>
- 全仓币币：cross<br>
- 全仓交割/永续/期权：cross<br>
- 逐仓交割/永续/期权：isolated<br>
<strong>组合保证金模式：</strong><br>
- 逐仓杠杆：isolated<br>
- 全仓币币：cross<br>
- 全仓交割/永续/期权：cross<br>
- 逐仓交割/永续/期权：isolated<br>
</aside>

<aside class="notice">
clOrdId<br>
clOrdId 是用户在 User ID 维度自定义的订单唯一标识符。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。<br>
clOrdId不能与当前所有的挂单的clOrdId重复
</aside>

<aside class="notice">
posSide<br>
持仓方向，买卖模式下此参数非必填，如果填写仅可以选择net；在开平仓模式下必填，且仅可选择 long 或 short。<br>
开平仓模式下，side和posSide需要进行组合<br>
开多：买入开多（side 填写 buy；  posSide 填写 long ）<br>
开空：卖出开空（side 填写 sell； posSide 填写 short ）<br>
平多：卖出平多（side 填写 sell；posSide 填写 long ）<br>
平空：买入平空（side 填写 buy； posSide 填写 short ）<br>
组合保证金模式：交割和永续仅支持买卖模式
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
optimal_limit_ioc：市价委托，立即成交并取消剩余，仅适用于交割合约和永续合约。
</aside>

<aside class="notice">
sz<br>
交易数量，表示要购买或者出售的数量。<br>
当币币/币币杠杆以限价买入和卖出时，指交易货币数量。<br>
当币币杠杆以市价买入时，指计价货币的数量。<br>
当币币杠杆以市价卖出时，指交易货币的数量。<br>
对于币币市价单，单位由 tgtCcy 决定<br>
当交割、永续、期权买入和卖出时，指合约张数。
</aside>

<aside class="notice">
reduceOnly<br>
只减仓，下单时，此参数设置为 true 时，表示此笔订单具有减仓属性，只会减少持仓数量，不会增加新的持仓仓位<br>
对于同一杠杆产品，所有反方向挂单的币数加上当前只减仓下单数量，不能超过仓位资产；负债还完后，如果还有剩余的委托数量，不会反向开仓，而是会进行币币交易。<br>
对于同一交割/永续产品，当前只减仓下单张数，加上价格时间优先于当前只减仓下单的只减仓挂单张数总和，不能超过持仓数量<br>
仅适用于`合约模式`和`跨币种保证金模式`<br>
仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>
注意：交割和永续合约在开平仓模式下，所有的平仓单都有只减仓逻辑，不受该字段传值的影响。
</aside>

<aside class="notice">
tgtCcy<br>
市价单委托数量`sz`的单位：仅适用于币币市价下单交易。<br>
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
强制自成交保护<br>
交易系统会以母账户维度实施强制自成交保护，同一母账户下所有账户，包括母账户本身和所有子账户，都无法进行自成交。默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。用户亦可以通过下单接口的stpMode参数指定订单的STP模式。<br>
强制自成交保护不会导致延迟。<br>
有三种STP模式。STP模式始终基于taker订单中的配置。<br>
1.Cancel Maker：这是默认的STP模式，系统撤Maker订单以防止自成交。然后，taker订单会基于深度继续和下一个订单成交。<br>
2.Cancel Taker：撤Taker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后撤单。FOK订单会确保完全成交和自成交保护。<br>
3.Cancel Both：撤Taker和Maker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后Taker订单的剩余数量和第一个自我Maker订单被取消。此模式不支持FOK订单。<br>
</aside>

<aside class="notice">
isElpTakerAccess:true订单限速<br>
- 50个/2s，限制维度为 User ID + Instrument ID<br>
- 该限速会在 REST 和 WebSocket 的下单及批量下单接口中共享
</aside>
