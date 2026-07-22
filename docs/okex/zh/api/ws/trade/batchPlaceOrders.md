---
site: okex
lang: zh
name: WS / 批量下单
type: guide
---

### WS / 批量下单 [order-book-trading-trade-ws-place-multiple-orders]

批量进行下单操作，每次可批量交易不同类型的产品，最多可下单20个<br><br>

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：300个/2s 
#### 跟单交易带单员带单产品的限速：4个/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`下单`限速中。
</aside>

<aside class="notice">
同`批量下单` REST API 共享限速
</aside>

> 请求示例

```shell
{
	"id": "1513",
	"op": "batch-orders",
	"args": [{
		"side": "buy",
		"instIdCode": 123456,
		"tdMode": "isolated",
		"ordType": "market",
		"sz": "100"
	}, {
		"side": "buy",
		"instIdCode": 654321,
		"tdMode": "isolated",
		"ordType": "limit",
		"sz": "1",
		"px": "20000"
	}]
}
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                                                                                                        |
|:---------------| :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。                                                                                                                                                                                                                                                                         |
| op             | String | 是       | 支持的业务操作，如 `batch-orders`                                                                                                                                                                                                                                                                                                                                  |
| args           | Array of objects  | 是       | 请求参数                                                                                                                                                                                                                                                                                                                                                      |
| > instIdCode   | Integer   | 是       | 产品唯一标识代码。   |
| > tdMode       | String  | 否       | 交易模式<br>保证金模式      `cross`：全仓  `isolated`：逐仓 <br>非保证金模式  `cash`：现金<br>`spot_isolated`：现货逐仓(仅适用于现货带单) ，现货带单时，`tdMode` 的值需要指定为`spot_isolated`<br>注意：`isolated` 在跨币种保证金模式和组合保证金模式下不可用。 <br><br><font color="red">事件合约对应交易产品仅支持`isolated`逐仓下单</font>                                                                                                                                                                                                                  |
| > ccy          | String  | 否       | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                                                                                                                                                                                                                                                               |
| > clOrdId      | String  | 否       | 用户提供的订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                                                                                                      |
| > tag          | String  | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-16位之间。                                                                                                                                                                                                                                                                                                           |
| > side         | String  | 是       | 订单方向， `buy`  `sell`                                                                                                                                                                                                                                                                                                                                       |
| > posSide      | String  | 否       | 持仓方向 <br>在买卖模式下，默认 `net`<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`，仅适用于`交割/永续`                                                                                                                                                                                                                                                                                  |
| > ordType      | String  | 是       | 订单类型 <br>`market`：市价单，仅适用于`币币/杠杆/交割/永续` <br>`limit`：限价单<br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消单 <br>`ioc`：立即成交并取消剩余单 <br>`optimal_limit_ioc`：市价委托立即成交并取消剩余（仅适用交割、永续）<br>`mmp`：做市商保护(仅适用于组合保证金账户模式下的期权订单)<br>`mmp_and_post_only`：做市商保护且只做maker单(仅适用于组合保证金账户模式下的期权订单) <br>`elp`：流动性增强计划订单                                                                                                    |
| > sz           | String  | 是       | 委托数量                                                                                                                                                                                                                                                                                                                                                      |
| > px           | String   | 可选     | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`、`mmp`、`mmp_and_post_only`类型的订单<br>期权下单时，px/pxUsd/pxVol 只能填一个                                                                                                                                                                                                                                                     |
| > speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| > outcome | String | 可选 | 用户交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，且为必填 |
| > pxUsd        | String | 可选 | 以USD价格进行期权下单 <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                                                                                                                              |
| > pxVol        | String | 可选 | 以隐含波动率进行期权下单，例如 1 代表 100% <br>仅适用于期权 <br>期权下单时 px/pxUsd/pxVol 必填一个，且只能填一个                                                                                                                                                                                                                                                                                 |
| > reduceOnly   | Boolean | 否       | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                                                                                                                                                                                                |
| > tgtCcy       | String | 否          | 币币市价单委托数量`sz`的单位<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                                                                                                                                                                                                                                  |
| > banAmend     | Boolean | 否    | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                                                                                                                                                                                                                                                                   |
| > pxAmendType  | String           | 否       | 订单价格修正类型<br>`0`：当`px`超出价格限制时，不允许系统修改订单价格<br>`1`：当`px`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| > tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| > slippagePct      | String | 否       | 币币、币币杠杆市价单（`tgtCcy` 为到手币种：买单为 `base_ccy`，卖单为 `quote_ccy`）的最大可接受滑点。<br>取值范围：`0` 至 `0.05`（即 0% 至 5%，含边界），以百分比形式表示时最多保留 2 位小数，例如 `0.01`（1%）和 `0.0123`（1.23%）合法；`0.01234`（1.234%）将被拒绝。<br>不填或为空时，默认为 `0.00%`。<br>不支持改单修改滑点，如需调整请撤单重新提交。<br>仅适用于币币和币币杠杆的市价单。 |
| > stpMode      | String | 否 | 自成交保护模式 <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both不支持FOK <br><br>默认使用账户层面的acctStpMode进行下单，该字段的默认值为`cancel_maker`，用户可通过母账户登录网页修改该配置；用户亦可以通过下单接口的stpMode参数指定订单的STP模式。|
| > isElpTakerAccess | Boolean | 否 | 是否作为 taker 吃单 ELP<br>`true`：该请求能吃单 ELP，但会被施加延迟<br>`false`：该请求不能吃单 ELP，并且没有延迟<br><br>默认值为`false`，`true`仅适用于ioc订单 |
| expTime        | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                                                                                                  |

> 全部成功返回示例

```json
{
	"id": "1513",
	"op": "batch-orders",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "",
		"ordId": "12344",
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

> 部分成功返回示例

```json
{
	"id": "1513",
	"op": "batch-orders",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "",
		"ordId": "",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "51008",
      	"sMsg": "Order failed. Insufficient USDT balance in account",
      	"subCode": "1000"
	}],
	"code": "2",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 全部失败返回示例

```json
{
	"id": "1513",
	"op": "batch-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "",
		"tag": "",
		"ts":"1695190491421",
		"sCode": "51008",
      	"sMsg": "Order failed. Insufficient USDT balance in account",
      	"subCode": "1000"
	}, {
		"clOrdId": "oktswap7",
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
	"id": "1513",
	"op": "batch-orders",
	"data": [],
	"code": "60013",
	"msg": "Invalid args",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| 参数 | 类型   | 描述             |
| :--- | :----- | :--------------- |
| id   | String | 消息的唯一标识   |
| op   | String | 业务操作   |
| code | String | 代码             |
| msg  | String | 消息             |
| data | Array of objects  | 请求成功后返回的数据 |
| > ordId     | String   | 订单ID                           |
| > clOrdId   | String   | 由用户设置的订单ID |
| > tag       | String   | 订单标签                         |
| > ts    | String  | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。  |
| > sCode     | String   | 订单状态码，0 代表成功 |
| > sMsg      | String   | 事件执行失败或成功时的msg        |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">
在组合保证金账户模式下，或者全部成功，或者全部失败。
</aside>

<aside class="notice">
clOrdId<br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有挂单和当前请求中的clOrdId重复。
</aside>

<aside class="notice">
isElpTakerAccess:true订单限速<br>
- 50个/2s，限制维度为 User ID + Instrument ID<br>
- 该限速会在 REST 和 WebSocket 的下单及批量下单接口中共享
</aside>
