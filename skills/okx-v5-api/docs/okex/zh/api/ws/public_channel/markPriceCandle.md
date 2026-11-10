---
site: okex
lang: zh
name: 标记价格K线频道
type: guide
---

### 标记价格K线频道 [public-data-websocket-mark-price-candlesticks-channel]

获取标记价格的K线数据，推送频率最快是间隔1秒推送一次数据。

#### URL Path

/ws/v5/business

> 请求示例

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [{
        "channel": "mark-price-candle1D",
        "instId": "BTC-USD-190628"
    }]
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
          "channel": "mark-price-candle1D",
          "instId": "BTC-USD-190628"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                         |
| :-------- | :----- | :------- | :----------------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`          |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                           |
| > channel | String | 是       | 频道名<br>`mark-price-candle3M` <br>`mark-price-candle1M` <br>`mark-price-candle1W` <br>`mark-price-candle1D`<br>`mark-price-candle2D`<br>`mark-price-candle3D` <br>`mark-price-candle5D`<br>`mark-price-candle12H` <br>`mark-price-candle6H` <br>`mark-price-candle4H`<br>`mark-price-candle2H`<br>`mark-price-candle1H` <br>`mark-price-candle30m`<br>`mark-price-candle15m` <br>`mark-price-candle5m` <br>`mark-price-candle3m` <br>`mark-price-candle1m` <br>`mark-price-candle3Mutc`<br>`mark-price-candle1Mutc` <br>`mark-price-candle1Wutc`<br>`mark-price-candle1Dutc`<br>`mark-price-candle2Dutc`<br>`mark-price-candle3Dutc`<br>`mark-price-candle5Dutc` <br>`mark-price-candle12Hutc`<br>`mark-price-candle6Hutc`  |
| > instId  | String | 是       | 产品ID                                                       |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "mark-price-candle1D",
        "instId": "BTC-USD-190628"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"mark-price-candle1D\", \"instId\" : \"BTC-USD-190628\"}]}",
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
| code      | String | 否       | 错误码                                                       |
| msg       | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
    "arg": {
        "channel": "mark-price-candle1D",
        "instId": "BTC-USD-190628"
    },
    "data": [
        ["1597026383085", "3.721", "3.743", "3.677", "3.708", "0"],
        ["1597026383085", "3.731", "3.799", "3.494", "3.72", "1"]
    ]
}
```
#### 推送数据参数

| 参数名    | 类型   | 描述                                                  |
| :-------- | :----- | :---------------------------------------------------- |
| arg     | Object | 订阅成功的频道                                        |
| > channel | String | 频道名                                                |
| > instId  | String | 产品ID                                                |
| data    | Array of Arrays  | 订阅的数据                                            |
| > ts       | String | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > o        | String | 开盘价格                                              |
| > h        | String | 最高价格                                              |
| > l        | String | 最低价格                                              |
| > c        | String | 收盘价格                                              |
| > confirm     | String  | K线状态 <br>`0` 代表 K 线未完结，`1` 代表 K 线已完结。 |
