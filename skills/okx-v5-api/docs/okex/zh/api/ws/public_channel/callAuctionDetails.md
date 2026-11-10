---
site: okex
lang: zh
name: WS / 集合竞价信息频道
type: guide
---

### WS / 集合竞价信息频道 [order-book-trading-market-data-ws-call-auction-details-channel]

获取集合竞价相关信息

#### URL Path

/ws/v5/public

> 请求示例
> 

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "call-auction-details",
        "instId": "ONDO-USDC"
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
    args = [{
        "channel": "call-auction-details",
        "instId": "ONDO-USDC"
    }]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数        | 类型 | 是否必须 | 描述 |
|-----------| --- | --- | --- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是 | 操作<br>`subscribe`<br>`unsubscribe` |
| args      | Array of objects | 是 | 请求订阅的频道列表 |
| > channel | String | 是 | 频道名<br>`call-auction-details` |
| > instId  | String | 是 | 产品ID |

> 成功返回示例
> 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "call-auction-details",
      "instId": "ONDO-USDC"
    },
  "connId": "a4d3ae55"
}

```

> 失败返回示例
> 

```json
{
  "id": "1512",
  "event": "error",
  "code": "60012",
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"call-auction-details\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}

```

#### 返回参数

| 参数 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| id     | String | 否       | 消息的唯一标识 |
| event | String | 是 | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | Object | 否 | 订阅的频道 |
| > channel | String | 是 | 频道名 |
| > instId | String | 是 | 产品ID |
| code | String | 否 | 错误码 |
| msg | String | 否 | 错误消息 |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例
> 

```json
{
  "arg": {
    "channel": "call-auction-details",
    "instId": "ONDO-USDC"
  },
  "data": [
        {
            "instId": "ONDO-USDC",
            "unmatchedSz": "9988764",
            "eqPx": "0.6",
            "matchedSz": "44978",
            "state": "continuous_trading",
            "auctionEndTime": "1726542000000",
            "ts": "1726542000007"
        }
  ]
}

```

#### 推送数据参数

| 参数名       | 类型 | 描述 |
|-----------| --- | --- |
| arg       | Object | 订阅成功的频道 |
| > channel | String | 频道名 |
| > instId  | String | 产品ID |
| data      | Array of objects | 订阅的数据 |
| > instId | String | 产品ID |
| > eqPx | String | 均衡价格 |
| > matchedSz | String | 买卖双边的匹配数量，单位为交易货币 |
| > unmatchedSz | String | 未匹配数量 |
| > auctionEndTime | String | 集合竞价结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > state | String | 交易状态<br>`call_auction`：集合竞价<br>`continuous_trading`：连续交易 |
| > ts | String | 数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
在集合竞价期间，用户可以获取均衡价格、匹配数量、未匹配数量和集合竞价结束时间的更新。数据大约每秒更新一次。当集合竞价结束时，该频道将推送最后一条消息，返回实际开盘价、匹配数量和未匹配数量，交易状态state为`continuous_trading`。
</aside>
