---
site: us
lang: zh
name: WS / 深度频道
type: guide
---

### WS / 深度频道 [order-book-trading-market-data-ws-order-book-channel]

获取深度数据。在提前挂单阶段，best ask的价格有机会低于best bid。`books`是400档频道，`books5`是5档频道， `bbo-tbt`是先1档后实时推送的频道，`bbo-rpi-tbt`是仅含RPI订单、先1档后实时推送的频道，`books-l2-tbt`是先400档后实时推送的频道，`books50-l2-tbt`是先50档后实时推的频道；

- `books` 首次推400档快照数据，以后增量推送，每100毫秒推送一次变化的数据<br>
- `books-elp`（已弃用，请使用 `books-rpi`）仅推送ELP订单，首次推400档快照数据，以后增量推送，每100毫秒推送一次变化的数据<br>
- `books-rpi`：合并有机和 RPI 深度。初始全量推送 400 档，之后每 100ms 推送增量。无 `checksum`，排序依赖 `seqId`/`prevSeqId`。每个 `asks`/`bids` 元素为 `[price, totalQty, nonRpiQty, count]`。取代 `books-elp`。
- `books5` 首次推5档快照数据，以后定量推送，每100毫秒当5档快照数据有变化推送一次5档数据<br>
- `bbo-tbt` 首次推1档快照数据，以后定量推送，每10毫秒当1档快照数据有变化推送一次1档数据<br>
- `bbo-rpi-tbt` 仅推送RPI订单，首次推1档快照数据，以后定量推送，每10毫秒当1档快照数据有变化推送一次1档数据<br>
- `books-l2-tbt` 首次推400档快照数据，以后增量推送，每10毫秒推送一次变化的数据<br>
- `books50-l2-tbt` 首次推50档快照数据，以后增量推送，每10毫秒推送一次变化的数据
- 单个连接、交易产品维度，深度频道的推送顺序固定为：bbo-tbt -> bbo-rpi-tbt -> books-l2-tbt -> books50-l2-tbt -> books -> books-elp -> books-rpi -> books5。
- 在相同连接下，用户将无法为相同交易产品同时订阅 `books-l2-tbt` 以及 `books50-l2-tbt/books`频道
  - 更多细节，请参阅更新日志 [2024-07-17] (/docs-v5/log_zh/#2024-07-17)

<aside class="notice">
books-l2-tbt400档深度频道，只允许交易手续费等级VIP4及以上的API用户订阅，其他用户接入将收到错误码64003。<br>
books50-l2-tbt50档深度频道，只允许交易手续费等级VIP4及以上的API用户订阅，其他用户接入将收到错误码64003。<br>
</aside>
身份认证参考[登录](/docs-v5/zh/#overview-websocket-login)功能 

#### 服务地址

/ws/v5/public

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "books",
		"instId": "BTC-USDT"
	}]
}
```

```python

import asyncio

from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/public")
    await ws.start()
    args = [
      {
        "channel": "books",
        "instId": "BTC-USDT"
      }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                                            |
| :-------- | :----- | :------- |:------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                            |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                                                     |
| > channel | String | 是       | 频道名<br>`books`<br>`books5`<br>`bbo-tbt`<br>`bbo-rpi-tbt`<br>`books-l2-tbt`<br>`books50-l2-tbt` |
| > instId  | String | 是       | 产品ID                                                                          |

> 返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "books",
		"instId": "BTC-USDT"
	},
	"connId": "a4d3ae55"
}
```

> 失败示例

```json
{
  	"id": "1512",
	"event": "error",
	"code": "60012",
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"books\", \"instId\" : \"BTC-USD-191227\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数      | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg     | Object | 否       | 订阅的频道                                                   |
| > channel | String | 是       | 频道名                                                       |
| > instId  | String | 是       | 产品ID                                                       |
| msg       | String | 否       | 错误消息                                                     |
| code      | String | 否       | 错误码                                                       |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例 ：全量

```json
{
	"arg": {
		"channel": "books",
		"instId": "BTC-USDT"
	},
	"action": "snapshot",
	"data": [{
		"asks": [
			["8476.98", "415", "0", "13"],
			["8477", "7", "0", "2"],
			["8477.34", "85", "0", "1"],
			["8477.56", "1", "0", "1"],
			["8505.84", "8", "0", "1"],
			["8506.37", "85", "0", "1"],
			["8506.49", "2", "0", "1"],
			["8506.96", "100", "0", "2"]
		],
		"bids": [
			["8476.97", "256", "0", "12"],
			["8475.55", "101", "0", "1"],
			["8475.54", "100", "0", "1"],
			["8475.3", "1", "0", "1"],
			["8447.32", "6", "0", "1"],
			["8447.02", "246", "0", "1"],
			["8446.83", "24", "0", "1"],
			["8446", "95", "0", "3"]
		],
		"ts": "1597026383085",
		"checksum": 0,
		"prevSeqId": -1,
      	"seqId": 123456
	}]
}
```

> 推送示例：增量

```json
{
	"arg": {
		"channel": "books",
		"instId": "BTC-USDT"
	},
	"action": "update",
	"data": [{
		"asks": [
			["8476.98", "415", "0", "13"],
			["8477", "7", "0", "2"],
			["8477.34", "85", "0", "1"],
			["8477.56", "1", "0", "1"],
			["8505.84", "8", "0", "1"],
			["8506.37", "85", "0", "1"],
			["8506.49", "2", "0", "1"],
			["8506.96", "100", "0", "2"]
		],
		"bids": [
			["8476.97", "256", "0", "12"],
			["8475.55", "101", "0", "1"],
			["8475.54", "100", "0", "1"],
			["8475.3", "1", "0", "1"],
			["8447.32", "6", "0", "1"],
			["8447.02", "246", "0", "1"],
			["8446.83", "24", "0", "1"],
			["8446", "95", "0", "3"]
		],
		"ts": "1597026383085",
		"checksum": 0,
		"prevSeqId": 123456,
      	"seqId": 123457
	}]
}
```

#### 推送数据参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| arg      | Object   | 订阅成功的频道                                               |
| > channel   | String   | 频道名                                                       |
| > instId    | String   | 产品ID                                                       |
| action    | String   | 推送数据动作，增量推送数据还是全量推送数据<br>`snapshot`：全量  <br>` update`：增量 |
| data     | Array of objects    | 订阅的数据                                                   |
| > asks      | Array of Arrays    | 卖方深度<br>对于 `bbo-rpi-tbt`，仅返回RPI订单，包含隐藏的RPI订单。卖方无RPI订单时返回 `[]`。                                                     |
| > bids      | Array of Arrays    | 买方深度<br>对于 `bbo-rpi-tbt`，仅返回RPI订单，包含隐藏的RPI订单。买方无RPI订单时返回 `[]`。                                                     |
| > ts        | String   | 数据更新时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` <br>例外： 对于`bbo-tbt` 和 `bbo-rpi-tbt` 频道，`ts` 为撮合引擎触发时的时间戳 |
| > checksum  | Integer  | <del>检验和</del>（已弃用）。该字段仍会在 `books`、`books-l2-tbt`、`books50-l2-tbt` 推送中保留，但其值固定为 `0`，不应再用于数据完整性校验。请改用 `seqId/prevSeqId` 校验数据的连续性和准确性。 |
| > prevSeqId | Integer | 上一个推送的序列号。仅适用 `books`，`books-l2-tbt`，`books50-l2-tbt` |
| > seqId     | Integer | 推送的序列号 （下方注解）        |

<aside class="notice">
asks和bids值数组举例说明： ["411.8", "10", "0", "4"]<br>
- 411.8为深度价格<br>
- 10为此价格的数量 （合约交易为张数，现货/币币杠杆为交易币的数量<br>
- 0该字段已弃用(始终为0)<br>
- 4为此价格的订单数量
</aside>

<aside class="notice">
如果需要订阅多个50或400档频道，建议通过多个链接进行订阅，每个链接低于30条频道。
</aside>

<aside class="notice">
集合竞价期间，深度数据大约每秒更新一次
</aside>

<aside class="notice">
`books/books5/bbo-tbt/books-l2-tbt/books50-l2-tbt`不包含ELP订单<br>
`books-elp`仅返回 ELP 订单，包含有效部分及无效部分（无效部分指 ELP 买单价格高于非 ELP 订单最佳买单价；或 ELP 卖单价格低于非 ELP 订单最佳卖单价）。用户需根据非 ELP 订单的最佳买/卖价区分有效部分和无效部分。
</aside>

<aside class="notice">
`books-rpi` 仅返回可见的 RPI 订单。<br>
`bbo-rpi-tbt` 仅返回 RPI 订单，包含可见部分及隐藏部分。隐藏部分指价格高于有机最佳卖价的 RPI 买单、价格低于有机最佳买价的 RPI 卖单，或在有机买卖价差内相互交叉的 RPI 买单和卖单。用户可根据 `bbo-tbt` 的有机最佳买/卖价区分可见部分和隐藏部分。<br>
`bbo-rpi-tbt` 的 asks 和 bids 值数组举例说明：["111.06", "55", "0", "2"]<br>
- 111.06为深度价格<br>
- 55为此价格的 RPI 订单总量（合约交易为张数，现货/币币杠杆为交易币的数量）<br>
- 0该字段已弃用(始终为0)<br>
- 2为此价格的 RPI 订单个数
</aside>

#### 序列号

`seqId`是交易所行情的一个序号。如果用户通过多个websocket连接同一频道，收到的序列号会是相同的。每个`instId`对应一套。用户可以使用在增量推送频道的`prevSeqId`和`seqId`来构建消息序列。这将允许用户检测数据包丢失和消息的排序。正常场景下`seqId`的值大于`prevSeqId`。新消息中的`prevSeqId`与上一条消息的`seqId`匹配。最小序列号值为0，除了快照消息的`prevSeqId`为-1。<br>

异常情况：<br>
1. 如果一段时间内（约 60 秒）没有深度更新，对于定量推送频道，OKX 会推送最近的一条更新，对于增量推送频道，OKX将发一条消息`'asks': [], 'bids': []`以通知用户连接是正常的。推送的`seqId`跟上一条信息的一样，`prevSeqId`等于`seqId`。
2. 序列号可能由于维护而重置，在这种情况下，用户将收到一条`seqId`小于`prevSeqId`的增量消息。随后的消息将遵循常规的排序规则。

##### 示例
1. 快照推送：`prevSeqId = -1`，`seqId = 10`
2. 增量推送1（正常更新）：`prevSeqId = 10`，`seqId = 15`
3. 增量推送2（无更新）：`prevSeqId = 15`，`seqId = 15`
4. 增量推送3（序列重置）：`prevSeqId = 15`，`seqId = 3`
5. 增量推送4（正常更新）：`prevSeqId = 3`，`seqId = 5`

> bbo-tbt 频道推送示例

```json
{
  "arg": {
    "channel": "bbo-tbt",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        [
          "111.06","55154","0","2"
        ]
      ],
      "bids": [
        [
          "111.05","57745","0","2"
        ]
      ],
      "ts": "1670324386802",
	  "seqId": 363996337
    }
  ]
}
```

> bbo-rpi-tbt 频道推送示例

```json
{
  "arg": {
    "channel": "bbo-rpi-tbt",
    "instId": "BTC-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        [
          "67855.2","0.5","0","1"
        ]
      ],
      "bids": [
        [
          "67858.1","1.2","0","2"
        ]
      ],
      "ts": "1792022400010",
      "seqId": 363996337
    }
  ]
}
```

> books5 频道推送示例

```json
{
  "arg": {
    "channel": "books5",
    "instId": "BCH-USDT-SWAP"
  },
  "data": [
    {
      "asks": [
        ["111.06","55154","0","2"],
        ["111.07","53276","0","2"],
        ["111.08","72435","0","2"],
        ["111.09","70312","0","2"],
        ["111.1","67272","0","2"]],
      "bids": [
        ["111.05","57745","0","2"],
        ["111.04","57109","0","2"],
        ["111.03","69563","0","2"],
        ["111.02","71248","0","2"],
        ["111.01","65090","0","2"]],
      "instId": "BCH-USDT-SWAP",
      "ts": "1670324386802",
	  "seqId": 363996337
    }
  ]
}
```
