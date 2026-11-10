---
site: okex
lang: zh
name: K线频道
type: guide
---

### K线频道 [spread-trading-websocket-public-channel-candlesticks-channel]

该频道使用业务WebSocket，不需鉴权。

获取K线数据，推送频率最快是间隔1秒推送一次数据。

#### URL Path

/ws/v5/business

> 请求示例

```shell
{
   "id": "1512",
   "op":"subscribe",
   "args":[
      {
         "channel":"sprd-candle1D",
         "sprdId":"BTC-USDT_BTC-USDT-SWAP"
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
         "channel":"sprd-candle1D",
         "sprdId":"BTC-USDT_BTC-USDT-SWAP"
      }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数名       | 类型               | 是否必须 | 描述 |
|-----------|------------------| --- | --- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String           | 是 | 操作<br>`subscribe` <br>`unsubscribe` |
| args      | Array of objects | 是 | 请求订阅的频道列表 |
| > channel | String           | 是 | 频道名<br>`sprd-candle3M` `sprd-candle1M` <br>`sprd-candle1W` <br>`sprd-candle1D` `sprd-candle2D` `sprd-candle3D` `sprd-candle5D` <br>`sprd-candle12H` `sprd-candle6H` `sprd-candle4H` `sprd-candle2H` `sprd-candle1H` <br>`sprd-candle30m` `sprd-candle15m` `sprd-candle5m` `sprd-candle3m` `sprd-candle1m` <br>`sprd-candle3Mutc` `sprd-candle1Mutc` `sprd-candle1Wutc` `sprd-candle1Dutc` `sprd-candle2Dutc` `sprd-candle3Dutc` `sprd-candle5Dutc` `sprd-candle12Hutc` `sprd-candle6Hutc` |
| > sprdId  | String           | 是 | Spread ID |

> 成功返回示例

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-candle1D",
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-candle1D\", \"instId\" : \"BTC-USD-191227\"}]}",
  "connId": "a4d3ae55"
}

```

#### 返回参数

| 参数名     | 类型 | 是否必须 | 描述 |
|---------| --- | --- | --- |
| id     | String | 否       | 消息的唯一标识 |
| event   | String | 是 | 事件 <br>`subscribe` <br>`unsubscribe` <br>`error` |
| arg     | Object | 否 | 订阅的频道 |
| channel | String | 是 | 频道名 |
| sprdId  | String | 是 | Spread ID |
| code    | String | 否 | 错误码 |
| msg     | String | 否 | 错误消息 |

> 推送示例

```json
{
  "arg": {
    "channel": "sprd-candle1D",
    "sprdId": "BTC-USDT_BTC-USD-SWAP"
  },
  "data": [
    [
      "1597026383085",
      "8533.02",
      "8553.74",
      "8527.17",
      "8548.26",
      "45247",
      "0"
    ]
  ]
}

```

#### 推送数据参数

| 参数名 | 类型 | 描述 | 
| --- | --- | --- |
| arg | Object | 订阅成功的频道 |  
| > channel | String | 频道名 |  
| > sprdId | String | Spread ID |  
| data | Array of Arrays | 订阅的数据 |  
| > ts | String | 开始时间，Unix时间戳的毫秒数格式，如 1597026383085 |  
| > o | String | 开盘价格 |  
| > h | String | 最高价格 |  
| > l | String | 最低价格 |  
| > c | String | 收盘价格 |  
| > vol | String | 交易量 |  
| > confirm | String | K线状态 <br>`0`：K线未完结 <br>`1`：K线已完结 |  

<aside class="notice">
返回值数组顺序分别为是： [ts,o,h,l,c,vol,confirm]
</aside>
