---
site: hongkong
lang: zh
name: WS / 交易频道
type: guide
---

### WS / 交易频道 [order-book-trading-market-data-ws-trades-channel]

获取最近的成交数据，有成交数据就推送，每次推送可能聚合多条成交数据。<br>
根据每个taker订单的不同成交价格，不同成交来源推送消息，并使用count字段表示聚合的订单匹配数量。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
  "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "trades",
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
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/public")
    await ws.start()
    args = [
        {
          "channel": "trades",
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

| 参数      | 类型   | 是否必须 | 描述                                                |
| :-------- | :----- | :------- | :-------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                  |
| > channel | String | 是       | 频道名<br>`trades`                                    |
| > instId  | String | 是       | 产品ID                                              |

> 成功返回示例

```json
{
  "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "trades",
		"instId": "BTC-USDT"
	},
  "connId": "a4d3ae55"
}
```

> 失败返回示例

```json
{
  "id": "1512",
	"event": "error",
	"code": "60012",
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"trades\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数      | 类型   | 是否必须 | 描述                                                        |
| :-------- | :----- | :------- |:----------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`             |
| arg     | Object | 否       | 订阅的频道                                                     |
| > channel | String | 是       | 频道名                                                       |
| > instId  | String | 是       | 产品ID                                                      |
| code      | String | 否       | 错误码                                                       |
| msg       | String | 否       | 错误消息                                                      |
| connId   | String   | 是  | WebSocket连接ID                                             |

> 推送示例

```json
{
  "arg": {
    "channel": "trades",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instId": "BTC-USDT",
      "tradeId": "130639474",
      "px": "42219.9",
      "sz": "0.12060306",
      "side": "buy",
      "ts": "1630048897897",
      "count": "3",
      "source": "0",
      "seqId": 1234
    }
  ]
}
```
#### 推送数据参数

| **参数名**   | **类型**     | **描述**                               |
|:----------|:-----------|:-------------------------------------|
| arg       | Object     | 订阅成功的频道                              |
| > channel | String     | 频道名                                  |
| > instId  | String     | 产品ID                                 |
| data      | Array of objects      | 订阅的数据                                |
| > instId  | String     | 产品ID，如 `BTC-USDT`                    |
| > tradeId | String     | 聚合的多笔交易中最新一笔交易的成交ID                  |
| > px      | String     | 成交价格                                 |
| > sz      | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| > side    | String     | 吃单方向<br>`buy`<br>`sell`              |
| > ts      | String     | 成交时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > count   | String     | 聚合的订单匹配数量                            |
| > source | String	 | 订单来源<br> `0`：普通订单<br>`1`：流动性增强计划订单 |
| > seqId     | Integer |  推送的序列号  |

<aside class="notice">
聚合功能说明：<br>
1. 系统将根据每个taker订单的不同成交价格，不同成交来源推送消息，并使用count字段表示聚合的订单匹配数量。<br>
2. tradeId是聚合的多笔交易中最新一笔交易的 ID。<br>
3. 当count = 1时，表示taker订单部分或完全成交时仅匹配了一个maker订单。<br>
4. 当count > 1时，表示taker订单以相同价格匹配了多个maker订单。例如，如果tradeId = 123，且count = 3，表示该消息聚合了tradeId = 123, 122, 121的成交。maker侧有多笔价格相同的订单被成交。<br>
5. 用户可以使用此数据与“全部交易”频道的数据进行对比。<br>
6. 深度及聚合交易数据仍按顺序发布。<br>
</aside>

<aside class="notice">
同时发生的不同交易推送数据的`seqId`可能相同。
</aside>
