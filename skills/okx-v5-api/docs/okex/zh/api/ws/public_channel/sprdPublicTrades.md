---
site: okex
lang: zh
name: 公共成交数据频道
type: guide
---

### 公共成交数据频道 [spread-trading-websocket-public-channel-public-trades-channel]

订阅`sprd-public-trades`获取最近的成交数据，有成交数据就推送，每次推送仅包含一条成交数据。

#### URL Path

/ws/v5/business

> 请求示例
> 

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-public-trades",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    }
  ]
}

```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "sprd-public-trades",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数 | 类型              | 是否必须 | 描述 |
| --- |-----------------| --- | --- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op | String          | 是 | 操作<br>`subscribe`<br>`unsubscribe` |
| args | Array of objects | 是 | 请求订阅的频道列表 |
| > channel | String          | 是 | 频道名<br>`sprd-public-trades` |
| > sprdId | String          | 是 | Spread ID |

> 成功返回示例
> 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
      "channel": "sprd-public-trades",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-public-trades\", \"instId\" : \"BTC-USD-191227\"}]}",
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
| > sprdId | String | 是 | Spread ID |
| code | String | 否 | 错误码 |
| msg | String | 否 | 错误消息 |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例
> 

```json
{
    "arg": {
        "channel": "sprd-public-trades",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    },
    "data": [
        {
            "sprdId": "BTC-USDT_BTC-USDT-SWAP",
            "tradeId": "2499206329160695808",
            "px": "-10",
            "sz": "0.001",
            "side": "sell",
            "ts": "1726801105519"
        }
    ]
}

```

#### 推送数据参数

| 参数名       | 类型              | 描述 |
|-----------|-----------------| --- |
| arg       | Object          | 订阅成功的频道 |
| > channel | String          | 频道名 |
| > sprdId  | String          | spread ID |
| data      | Array of objects | 订阅的数据 |
| > sprdId  | String          | spread ID |
| > tradeId | String          | 交易 ID |
| > px      | String          | 成交价格 |
| sz         | String          | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| > side    | String          | 成交方向 <br>`buy` <br>`sell` |
| > ts      | String          | 成交时间，Unix时间戳的毫秒数格式，如 1597026383085 |
