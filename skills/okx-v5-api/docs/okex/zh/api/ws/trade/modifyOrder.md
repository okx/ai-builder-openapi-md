---
site: okex
lang: zh
name: WS / 改单
type: guide
---

### WS / 改单 [order-book-trading-trade-ws-amend-order]

修改当前未成交的订单

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：60次/2s  
#### 跟单交易带单员带单产品的限速：4次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

该接口限速同时受到 [子账户限速] (/docs-v5/log_zh/#upcoming-changes-sub-account-rate-limit) 及 [基于成交比率的子账户限速] (/docs-v5/log_zh/#upcoming-changes-fill-ratio-based-sub-account-rate-limit) 限速规则的影响。

<aside class="notice">
同`改单` REST API 共享限速
</aside>

> 请求示例

```shell
{
	"id": "1512",
	"op": "amend-order",
	"args": [{
		"instIdCode": 123456,
		"ordId": "2510789768709120",
		"newSz": "2"
	}]
}
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                        |
| :----- | :----- | -------- |:------------------------------------------------------------------------------------------|
| id     | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。         |
| op     | String | 是       | 支持的业务操作，如 `amend-order`                                                                   |
| args   | Array of objects  | 是       | 请求参数                                                                                      |
| > instIdCode   | Integer   | 是       | 产品唯一标识代码   |
| > cxlOnFail | Boolean | 否       | 当订单修改失败时，该订单是否需要自动撤销。默认为`false`<br>`false`：不自动撤单<br>`true`：自动撤单                           |
| > ordId     | String  | 可选     | 订单ID<br>ordId和clOrdId必须传一个，若传两个，以 ordId 为主                                                |
| > clOrdId   | String  | 可选     | 用户提供的订单ID                                                                                 |
| > reqId     | String  | 否       | 用户提供的reqId<br>如果提供，那在返回参数中返回reqId，方便找到相应的修改请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| > newSz     | String  | 可选     | 请求修改的新数量，必须大于0。`newSz`和`newPx`不可同时为空。对于部分成交订单，该数量应包含已成交数量。                                |
| > newPx     | String  | 可选     | 修改后的新价格<br>修改的新价格期权改单时，newPx/newPxUsd/newPxVol 只能填一个，且必须与下单参数保持一致，如下单用px，改单时需使用newPx      |
| > speedBump | String | 可选 | 减速带<br>`1`：事件合约速度限制（延迟可能因市场情况调整，不提前通知）。对 `EVENTS` 产品的非只挂单操作为必填。 |
| > newPxUsd  | String | 可选 | 以USD价格进行期权改单 <br>仅适用于期权，期权改单时，newPx/newPxUsd/newPxVol 只能填一个                               |
| > newPxVol  | String | 可选 | 以隐含波动率进行期权改单，例如 1 代表 100% <br>仅适用于期权，期权改单时，newPx/newPxUsd/newPxVol 只能填一个                  |
| > pxAmendType  | String  | 否       | 订单价格修正类型<br>`0`：当`newPx`超出价格限制时，不允许系统修改订单价格<br>`1`：当`newPx`超出价格限制时，允许系统将价格修改为限制范围内的最优值<br>默认值为`0`         |
| > rpiTakerAccess | Boolean | 否 | 默认值为 `false`。<br>设为 `true` 时，改单后的订单可使用 RPI 流动性，适用于 `limit`、`market`、`fok`、`ioc` 订单。<br>改单时不会从原始订单继承，必须每次显式指定（省略则该次改单视为 `false`）。 |
| > rpiPxRound | Boolean | 否 | 默认值为 `false`。仅对 RPI 做市商订单（`ordType: rpi`）生效。设为 `true` 时，违反间距规则的价格将自动向外取整至最近的合规价位，而非被拒绝。对非 RPI 订单及 `OPTION`/`EVENTS` 无效。 |
| expTime | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085`                                                  |

> 成功返回示例

```json
{
	"id": "1512",
	"op": "amend-order",
	"data": [{
		"clOrdId": "",
		"ordId": "2510789768709120",
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

> 失败返回示例

```json
{
	"id": "1512",
	"op": "amend-order",
	"data": [{
		"clOrdId": "",
		"ordId": "2510789768709120",
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

> 格式错误返回示例

```json
{
	"id": "1512",
	"op": "amend-order",
	"data": [],
	"code": "60013",
	"msg": "Invalid args",
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
	
}
```

#### 返回参数 

| 参数     | 类型   | 描述                                                         |
| :------- | :----- | :----------------------------------------------------------- |
| id       | String | 消息的唯一标识                                               |
| op       | String | 业务操作                                                     |
| code     | String | 代码                                                         |
| msg      | String | 消息                                                         |
| data     | Array of objects  | 请求成功后返回的数据                                         |
| > ordId   | String | 订单ID                                                       |
| > clOrdId | String | 用户提供的订单ID                                             |
| > ts    | String  | 订单创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`。与订单频道中的 `cTime` 相同。  |
| > reqId   | String | 用户提供的reqId<br>如果用户在请求中提供reqId，则返回相应reqId |
| > sCode   | String | 订单状态码，0 代表成功                                       |
| > sMsg    | String | 订单状态消息                                                 |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | WebSocket 网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|
| outTime | String | WebSocket 网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`|

<aside class="notice"> 
  <p> 
    newSz : 当修改已经部分成交的订单时，新的委托数量必须大于等于已成交数量
  </p> 
</aside>

<aside class="notice">
  <p>
修改订单返回sCode等于0不能严格认为该订单已经被修改，只表示您的修改订单请求被系统服务器所接受，改单结果以订单频道推送的状态或者查询订单状态为准<br>
  </p>
</aside>
