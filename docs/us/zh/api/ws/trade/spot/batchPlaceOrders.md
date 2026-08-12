---
site: us
lang: zh
name: WS / 批量下单
type: guide
---

### WS / 批量下单 [order-book-trading-trade-ws-place-multiple-orders]

<aside class="notice">
该频道为私有频道，需验证后使用
</aside>

批量进行下单操作，每次可批量交易不同类型的产品，最多可下单20个<br><br>

#### 限速：300个/2s 
#### 限速规则：User ID + Instrument ID

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
		"instId": "BTC-USDT",
		"tdMode": "cash",
		"ordType": "market",
		"sz": "100"
	}, {
		"side": "buy",
		"instId": "BTC-USD-200925",
		"tdMode": "cash",
		"ordType": "limit",
		"sz": "1"
	}]
} 
```

#### 请求参数

| 参数名            | 类型       | 是否必须 | 描述                                                                                                                                                                                                                                                    |
|:---------------|:---------|:-----|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String   | 是    | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。                                                                                                                                                                    |
| op             | String   | 是    | 支持的业务操作<br>`batch-orders`                                                                                                                                                                                                                             |
| args           | Array of objects    | 是    | 请求参数                                                                                                                                                                                                                                                  |
| > instId       | String   | 是    | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                     |
| > tdMode       | String   | 否    | 交易模式<br>`cash`：现金                                                                                                                                                                                                                                     |
| > clOrdId      | String   | 否    | 用户提供的订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                 |
| > tag          | String   | 否    | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-16位之间。                                                                                                                                                                                                       |
| > side         | String   | 是    | 订单方向<br>`buy`<br>`sell`                                                                                                                                                                                                                               |
| > ordType      | String   | 是    | 订单类型<br>`market`：市价单 <br>`limit`：限价单<br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消单 <br>`ioc`：立即成交并取消剩余单 <br>`rpi`：Retail Price Improvement 订单<br>`elp`：流动性增强计划订单（已弃用，请使用 `rpi`。2026年10月31日前仍可使用。） |
| > sz           | String   | 是    | 委托数量                                                                                                                                                                                                                                                  |
| > px           | String   | 可选   | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`类型的订单                                                                                                                                                 |
| > tgtCcy       | String   | 否    | 币币市价单委托数量`sz`的单位<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`市价订单<br>默认买单为`quote_ccy`，卖单为`base_ccy`                                                                                                                                              |
| > banAmend     | Boolean  | 否    | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                                                                                                                                                               |
| > stpId        | String   | 否    | ~~自成交保护ID。来自同一个母账户配着同一个ID的订单不能自成交<br>用户自定义1<=x<=999999999的整数~~（已弃用）                                                                                                                                                                                           |
| > stpMode      | String   | 否    | 自成交保护模式<br>默认为 cancel maker <br>`cancel_maker`,`cancel_taker`, `cancel_both`<br>Cancel both不支持FOK                                                                                                                                  |
| > rpiTakerAccess | Boolean | 否 | 默认值为 `false`。<br>设为 `true` 时，订单可使用 RPI 流动性，适用于 `limit`、`market`、`fok`、`ioc` 订单。<br>当 `rpiTakerAccess` 为 `true` 时，减速带机制在下单和改单时均适用于所有 `ordType`，包括 `post_only`。<br>改单时不会从原始订单继承，必须每次显式指定（省略则该次改单视为 `false`）。<br>`isElpTakerAccess` 在 2026年10月31日前作为别名继续被接受。 |
| > rpiPxRound | Boolean | 否 | 默认值为 `false`。仅对 RPI 做市商订单（`ordType: rpi`）生效。设为 `true` 时，违反 RPI 做市商间距规则的价格将自动向外取整至最近的合规价位，而非被拒绝。对非 RPI 订单及 `OPTION`/`EVENTS` 无效。 |
| expTime        | String   | 否    | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                              |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |

> 全部成功返回示例

```json
{
	"id": "1513",
	"op": "batch-orders",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"tag": "",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "",
		"ordId": "12344",
		"tag": "",
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
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "",
		"ordId": "",
		"tag": "",
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
		"sCode": "51008",
		"sMsg": "Order failed. Insufficient USDT balance in account",
		"subCode": "1000"
	}, {
		"clOrdId": "oktswap7",
		"ordId": "",
		"tag": "",
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
| > sCode     | String   | 订单状态码，0 代表成功 |
| > sMsg      | String   | 事件执行失败或成功时的msg        |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice">
clOrdId<br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有挂单和当前请求中的clOrdId重复。
</aside>
