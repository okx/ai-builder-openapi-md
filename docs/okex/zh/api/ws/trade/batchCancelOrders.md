---
site: okex
lang: zh
name: WS / 批量撤单
type: guide
---

### WS / 批量撤单 [order-book-trading-trade-ws-cancel-multiple-orders]

批量进行撤单操作，每次可批量撤销不同类型的产品，最多撤销20个

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：300个/2s  

#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

<aside class="notice">
与其他限速按接口调用次数不同，该接口限速按订单的总个数限速。如果单次批量请求中只有一个元素，则算在单个`撤单`限速中。
</aside>

<aside class="notice">
同`批量撤单` REST API 共享限速
</aside>

> 请求示例

```shell
{
	"id": "1515",
	"op": "batch-cancel-orders",
	"args": [{
		"instIdCode": 123456,
		"ordId": "2517748157541376"
	}, {
		"instIdCode": 654321,
		"ordId": "2517748155771904"
	}]
}
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                                                         |
| :--- | :----- | -------- | :----------------------------------------------------------- |
| id   | String | 是       | 消息的唯一标识<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。|
| op   | String | 是       | 支持的业务操作，如  `batch-cancel-orders`                  |
| args | Array of objects  | 是       | 请求参数                                                     |
| > instIdCode   | Integer   | 是       | 产品唯一标识代码   |
| > ordId  | String   | 可选     | 订单ID<br>ordId和clOrdId必须传一个，若传两个，以ordId 为主 |
| > clOrdId | String   | 可选     | 用户提供的订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度要在1-32位之间。 |

> 全部成功返回示例

```json
{
	"id": "1515",
	"op": "batch-cancel-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "2517748157541376",
		"ts": "1695190491421",
		"sCode": "0",
		"sMsg": ""
	}, {
		"clOrdId": "oktswap7",
		"ordId": "2517748155771904",
		"ts": "1695190491421",
		"sCode": "0",
		"sMsg": ""
	}],
	"code": "0",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 部分成功的返回示例

```json
{
	"id": "1515",
	"op": "batch-cancel-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "2517748157541376",
		"ts": "1695190491421",
		"sCode": "0",
		"sMsg": ""
	}, {
		"clOrdId": "oktswap7",
		"ordId": "2517748155771904",
		"ts": "1695190491421",
		"sCode": "5XXXX",
		"sMsg": "order not exist"
	}],
	"code": "2",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 全部失败的返回示例

```json
{
	"id": "1515",
	"op": "batch-cancel-orders",
	"data": [{
		"clOrdId": "oktswap6",
		"ordId": "2517748157541376",
		"ts": "1695190491421",
		"sCode": "5XXXX",
		"sMsg": "order not exist"
	}, {
		"clOrdId": "oktswap7",
		"ordId": "2517748155771904",
		"ts": "1695190491421",
		"sCode": "5XXXX",
		"sMsg": "order not exist"
	}],
	"code": "1",
	"msg": "",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

> 格式错误示例

```json
{
	"id": "1515",
	"op": "batch-cancel-orders",
	"data": [],
	"code": "60013",
	"msg": "Invalid args",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| 参数 | 类型   | 描述           |
| :--- | :----- | :------------- |
| id   | String | 消息的唯一标识 |
| op   | String | 业务操作 |
| code | String | 代码           |
| msg  | String | 消息           |
| data | Array of objects | 请求成功后返回的数据 |
| > ordId     | String       | 订单ID                           |
| > clOrdId   | String       | 由用户设置的订单ID |
| > ts    | String  | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。  |
| > sCode     | String       | 订单状态码，0 代表成功 |
| > sMsg      | String       | 订单状态消息        |
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
