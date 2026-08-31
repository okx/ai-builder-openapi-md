---
site: okex
lang: zh
name: 指数K线频道
type: guide
---

### 指数K线频道 [public-data-websocket-index-candlesticks-channel]

获取指数的K线数据，推送频率最快是间隔1秒推送一次数据。

#### URL Path

/ws/v5/business

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "index-candle30m",
		"instId": "BTC-USD"
	}]
}
```

```python
import asyncio
from okx.websocket.WsPublicAsync import WsPublicAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPublicAsync(url="wss://wspap.okx.com:8443/ws/v5/business")
    await ws.start()
    args = [
        {
          "channel": "index-candle30m",
          "instId": "BTC-USD"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```
#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :-------- | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > channel | String | 是       | 频道名<br>`index-candle3M`<br>`index-candle1M`<br>`index-candle1W`<br>`index-candle1D`<br>`index-candle2D`<br>`index-candle3D`<br>`index-candle5D`<br>`index-candle12H`<br>`index-candle6H`<br>`index-candle4H`<br>`index -candle2H`<br>`index-candle1H`<br>`index-candle30m`<br>`index-candle15m`<br>`index-candle5m`<br>`index-candle3m`<br>`index-candle1m`<br>`index-candle3Mutc` <br>`index-candle1Mutc`<br>`index-candle1Wutc`<br>`index-candle1Dutc`<br>`index-candle2Dutc`<br>`index-candle3Dutc`<br>`index-candle5Dutc`<br>`index-candle12Hutc`<br>`index-candle6Hutc`|
| > instId  | String | 是       | 现货指数，如 `BTC-USD`<br>与 `uly` 含义相同。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "index-candle30m",
		"instId": "BTC-USD"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"index-candle30m\", \"instId\" : \"BTC-USD\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                          |
| :--------- | :----- | :------- | :-------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | `subscribe`      `unsubscribe` |
| arg      | Object | 否       | 订阅的频道                                    |
| > channel | String | 是       | 频道名                                        |
| > instId  | String | 否       | 现货指数                                        |
| code       | String | 否       | 错误码                                        |
| msg        | String | 否       | 错误消息                                      |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "index-candle30m",
		"instId": "BTC-USD"
	},
	"data": [
		["1597026383085", "3811.31", "3811.31", "3811.31", "3811.31","0"]
	]
}
```
#### 推送数据参数

| 参数名    | 类型   | 描述                                                  |
| :-------- | :----- | :---------------------------------------------------- |
| arg     | Object | 订阅成功的频道                                        |
| > channel | String | 频道名                                                |
| > instId  | String | 现货指数                                                |
| data    | Array of Arrays  | 订阅的数据                                            |
| > ts       | String | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > o        | String | 开盘价格                                              |
| > h        | String | 最高价格                                              |
| > l        | String | 最低价格                                              |
| > c        | String | 收盘价格                                              |
| > confirm     | String  | K线状态 <br>`0` 代表 K 线未完结，`1` 代表 K 线已完结。 |

<aside class="notice"> 
返回值数组顺分别为是：[ts,o,h,l,c,confirm]
</aside>
