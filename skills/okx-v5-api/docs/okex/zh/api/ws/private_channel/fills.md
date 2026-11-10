---
site: okex
lang: zh
name: WS / 成交频道
type: guide
---

### WS / 成交频道 [order-book-trading-trade-ws-fills-channel]

获取成交信息。该频道无首推，仅在订单簿成交相关事件触发时推送数据，tradeId > 0。

该频道仅适用于交易等级VIP4及以上的用户，其他用户接入将收到错误码64003。其他用户请使用[WS / 订单频道](/docs-v5/zh/#order-book-trading-trade-ws-order-channel)。

对于 `EVENTS`，无论实际订单是否为 YES 或 NO 方向，仅推送 YES 侧成交数据。

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例：单个

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "fills",
            "instId": "BTC-USDT-SWAP"
        }
    ]
}
```

```python
import asyncio

from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():

    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "fills",
            "instId": "BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> 请求示例

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "fills"
        }
    ]
}
```

```python
import asyncio

from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():

    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "fills"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数名       | 类型               | 是否必须 | 描述                                 |
|-----------|------------------|------|------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String           | 是    | 操作<br>`subscribe`<br>`unsubscribe` |
| args      | Array of objects | 是    | 订阅的频道                              |
| > channel | String           | 是    | 频道名<br>`fills`                     |
| > instId  | String           | 否    | 产品ID                               |

> 成功返回示例：单个

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "fills",
    "instId": "BTC-USDT-SWAP"
  },
  "connId": "a4d3ae55"
}

```

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "fills"
  },
  "connId": "a4d3ae55"
}

```

#### 返回参数

| 参数名       | 类型 | 是否必须 | 描述                                            |
|-----------| --- | --- |-----------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是 | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | 否 | 订阅的频道                                         |
| > channel | String | 是 | 频道名<br>`fills`                                |
| > instId  | String | 否 | 产品ID                                          |
| code      | String | 否 | 错误码                                           |
| msg       | String | 否 | 错误消息                                          |
| connId    | String | 是 | WebSocket连接ID                                 |

> 推送示例：单个

```json
{
    "arg": {
        "channel": "fills",
        "instId": "BTC-USDT-SWAP",
        "uid": "614488474791111"
    },
    "data":[
        {
            "instId": "BTC-USDT-SWAP",
            "fillSz": "100",
            "fillPx": "70000",
            "side": "buy",
            "ts": "1705449605015",
            "ordId": "680800019749904384",
            "clOrdId": "1234567890",
            "tradeId": "12345",
            "execType": "T",
            "count": "10"
        }
    ]
}

```

#### 推送数据参数

| 参数名        | 类型               | 描述                                          |
|------------|------------------|---------------------------------------------|
| arg        | Object           | 订阅成功的频道                                     |
| > channel  | String           | 频道名                                         |
| > uid      | String           | 用户标识                                        |
| > instId   | String           | 产品ID                                        |
| data       | Array of objects | 订阅的数据                                       |
| > instId   | String           | 产品ID                                        |
| > fillSz   | String           | 成交数量，若这笔成交有聚合，则成交数量为聚合后的数量                  |
| > fillPx   | String           | 成交价格                                        |
| > side     | String           | 订单方向<br>`buy`<br>`sell`                     |
| > ts       | String           | 成交时间                                        |
| > ordId    | String           | 订单ID                                        |
| > clOrdId     | String |  由用户设置的订单ID  |
| > tradeId  | String           | 成交ID<br>若为taker订单且有聚合，则为聚合的多笔交易中最新一笔交易的成交ID |
| > execType | String           | 流动性方向<br>`T`：taker<br>`M`：maker             |
| > count    | String           | 聚合的订单匹配数量                                   |

<aside class="notice"> 
- 该频道仅适用于交易等级VIP4及以上的用户，其他用户接入将收到错误码64003<br>
- 该频道只推送部分订单频道的信息，与大宗交易、价差速递相关的成交，强平、自动减仓等非订单簿事件不会通过该频道推送。用户应同时关注订单频道，对订单做最终确认<br>
- 该频道接收到成交推送时，账户余额、保证金、持仓等信息可能仍未发生变化<br>
- taker订单将根据不同成交价格进行聚合，有聚合时，count字段表示聚合的订单匹配数量，tradeId代表聚合的多笔交易中最新一笔交易的ID；maker订单不会聚合<br>
- 用户可以在下单时指定clOrdId，成交时会返回该字段。请注意，成交频道仅在用户输入的clOrdId符合带符号int64正整数格式（1-9223372036854775807, 2^63-1）时返回该字段；若用户未输入该字段，或clOrdId不符合格式要求，该字段将返回"0"。订单接口及频道将照常返回用户传入的clOrdId。所有请求及返回参数均为字符串类型。<br>
- 未来，该频道将施加连接数量限制，子账户维度，订阅成交频道的最大连接数为20个。我们建议用户始终低于限制使用该频道，以免限制上线后对策略造成影响
</aside>
