---
site: okex
lang: zh
name: WS / 批量改单
type: guide
---

### WS / 批量改单 [order-book-trading-trade-ws-amend-multiple-orders]

批量进行改单操作，每次可批量修改不同类型的产品，最多改20个

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：300个/2s 
#### 跟单交易带单员带单产品的限速：4个/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`修改订单`限速中。
</aside>

<aside class="notice">
同`批量改单` REST API 共享限速
</aside>

> 请求示例

```shell
{
	"id": "1513",
	"op": "batch-amend-orders",
	"args": [{
		"instIdCode": 123456,
		"ordId": "12345689",
		"newSz": "2"
	}, {
		"instIdCode": 123456,
		"ordId": "12344",
		"newSz": "2"
	}]
}
```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                                       |
|:------------| :----- | -------- |:-----------------------------------------------------------------------------------------|
| id          | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。        |
| op          | String | 是       | 支持的业务操作，如 `batch-amend-orders`                                                           |
| args        | Array of objects  | 是       | 请求参数                                                                                     |
| > instIdCode   | Integer   | 是       | 产品唯一标识代码   |
| > cxlOnFail | Boolean | 否       | 当订单修改失败时，该订单是否需要自动撤销。默认为`false`<br>`false`：不自动撤单<br>`true`：自动撤单                          |
| > ordId     | String  | 可选     | 订单ID<br>ordId 和 clOrdId 必须传一个，若传两个，以order id 为主                                          |
| > clOrdId   | String  | 可选     | 用户提供的订单ID                                                                                |
| > reqId     | String  | 否       | 用户提供的请求ID<br>如果提供，那在返回参数中返回reqId，方便找到相应的修改请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| > newSz     | String  | 可选     | 修改后的新数量，必须大于0。`newSz`和`newPx`不可同时为空。对于部分成交订单，该数量应包含已成交数量。                                |
| > newPx     | String  | 可选     | 修改后的新价格<br>修改的新价格期权改单时，newPx/newPxUsd/newPxVol 只能填一个，且必须与下单参数保持一致，如下单用px，改单时需使用newPx     |
| > speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| > newPxUsd  | String | 可选 | 以USD价格进行期权改单 <br>仅适用于期权，期权改单时，newPx/newPxUsd/newPxVol 只能填一个                              |
| > newPxVol  | String | 可选 | 以隐含波动率进行期权改单，例如 1 代表 100% <br>仅适用于期权，期权改单时，newPx/newPxUsd/newPxVol 只能填一个                 |
| > pxAmendType  | String  | 否       | 订单价格修正类型<br>`0`：当`newPx`超出价格限制时，不允许系统修改订单价格<br>`1`：当`newPx`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| expTime     | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                 |

> 全部成功返回示例

```json
{
	"id": "1513",
	"op": "batch-amend-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "12345689",
		"ts": "1695190491421",
		"reqId": "b12344",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "oktswap7",
		"ordId": "12344",
		"ts": "1695190491421",
		"reqId": "b12344",
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

> 全部失败返回示例

```json
{
	"id": "1513",
	"op": "batch-amend-orders",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"ts": "1695190491421",
		"reqId": "b12344",
		"sCode": "51008",
      	"sMsg": "Order failed. Insufficient USDT balance in account",
      	"subCode": "1000"
	}, {
		"clOrdId": "oktswap7",
		"ordId": "",
		"ts": "1695190491421",
		"reqId": "b12344",
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

> 部分成功返回示例

```json
{
	"id": "1513",
	"op": "batch-amend-orders",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"ts": "1695190491421",
		"reqId": "b12344",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""

	}, {
		"clOrdId": "oktswap7",
		"ordId": "",
		"ts": "1695190491421",
		"reqId": "b12344",
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

> 格式错误返回示例

```json
{
	"id": "1513",
	"op": "batch-amend-orders",
	"data": [],
	"code": "60013",
	"msg": "Invalid args",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| 参数名   | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| id       | String | 消息的唯一标识                                               |
| op       | String | 业务操作                                                     |
| code     | String | 代码                                                         |
| msg      | String | 消息                                                         |
| data     | Array of objects  | 请求成功后返回的数据                                         |
| > ordId   | String | 订单ID                                                       |
| > clOrdId | String | 由用户设置的订单ID                                           |
| > ts    | String  | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。  |
| > reqId   | String | 用户提供的请求ID<br>如果用户在请求中提供reqId，则返回相应reqId |
| > sCode   | String | 订单状态码，0 代表成功                                       |
| > sMsg    | String | 订单状态消息                                                 |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
