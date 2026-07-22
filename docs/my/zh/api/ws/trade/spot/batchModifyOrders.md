---
site: my
lang: zh
name: WS / 批量改单
type: guide
---

### WS / 批量改单 [order-book-trading-trade-ws-amend-multiple-orders]

<aside class="notice">
该频道为私有频道，需验证后使用
</aside>

批量进行改单操作，每次可批量修改不同类型的产品，最多改20个

#### 限速：300个/2s 
#### 限速规则：User ID + Instrument ID

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
		"instId": "BTC-USDT",
		"ordId": "12345689",
		"newSz": "2"
	}, {
		"instId": "BTC-USDT",
		"ordId": "12344",
		"newSz": "2"
	}]
}
```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                                       |
|:------------| :----- | -------- |:-----------------------------------------------------------------------------------------|
| id          | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。       |
| op          | String | 是       | 支持的业务操作<br>`batch-amend-orders`                                                          |
| args        | Array of objects  | 是       | 请求参数                                                                                     |
| > instId    | String  | 是       | 产品ID                                                                                     |
| > cxlOnFail | Boolean | 否       | 当订单修改失败时，该订单是否需要自动撤销。默认为`false`<br>`false`：不自动撤单<br>`true`：自动撤单                       |
| > ordId     | String  | 可选     | 订单ID<br>`ordId`和`clOrdId`必须传一个，若传两个，以`ordId`为主                                           |
| > clOrdId   | String  | 可选     | 用户提供的订单ID                                                                                |
| > reqId     | String  | 否       | 用户提供的请求ID<br>如果提供，那在返回参数中返回reqId，方便找到相应的修改请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| > newSz     | String  | 可选     | 修改后的新数量<br>`newSz`和`newPx`不可同时为空。对于部分成交订单，该数量应包含已成交数量。                                   |
| > newPx     | String  | 可选     | 修改后的新价格                                                                                  |
| expTime     | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                 |

> 全部成功返回示例

```json
{
	"id": "1513",
	"op": "batch-amend-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "12345689",
		"reqId": "b12344",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "oktswap7",
		"ordId": "12344",
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
		"reqId": "b12344",
		"sCode": "51008",
      	"sMsg": "Order failed. Insufficient USDT balance in account",
      	"subCode": "1000"
	}, {
		"clOrdId": "oktswap7",
		"ordId": "",
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
		"reqId": "b12344",
		"sCode": "0",
		"sMsg": "",
		"subCode": ""
	}, {
		"clOrdId": "oktswap7",
		"ordId": "",
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
| > reqId   | String | 用户提供的请求ID<br>如果用户在请求中提供reqId，则返回相应reqId |
| > sCode   | String | 订单状态码，0 代表成功                                       |
| > sMsg    | String | 订单状态消息                                                 |
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
