---
site: okex
lang: zh
name: 指数行情频道
type: guide
---

### 指数行情频道 [public-data-websocket-index-tickers-channel]

获取指数的行情数据。每100ms有变化就推送一次数据，否则一分钟推一次。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "index-tickers",
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
          "channel": "index-tickers",
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

| 参数      | 类型   | 是否必须 | 描述                                          |
| :-------- | :----- | :------- |:--------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | `subscribe`      `unsubscribe`              |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                   |
| > channel | String | 是       | 频道名<br>`index-tickers`                         |
| > instId  | String | 是       | 指数，以USD、USDT、BTC、USDC 为计价货币的指数，如 `BTC-USDT`<br>与 `uly` 含义相同。 |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "index-tickers",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"index-tickers\", \"instId\" : \"BTC-USDT\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                          |
| :--------- | :----- | :------- |:--------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | `subscribe`      `unsubscribe`    `error`   |
| arg      | Object | 否       | 订阅的频道                                       |
| > channel | String | 是       | 频道名<br>`index-tickers`                         |
| > instId  | String | 是       | 指数，以USD、USDT、BTC、USDC 为计价货币的指数，如 `BTC-USDT` |
| code       | String | 否       | 错误码                                         |
| msg        | String | 否       | 错误消息                                        |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "index-tickers",
		"instId": "BTC-USDT"
	},
	"data": [{
		"instId": "BTC-USDT",
		"idxPx": "0.1",
		"high24h": "0.5",
		"low24h": "0.1",
		"open24h": "0.1",
		"sodUtc0": "0.1",
		"sodUtc8": "0.1",
		"ts": "1597026383085"
	}]
}
```
#### 推送数据参数

| 参数名    | 类型   | 描述                                                         |
| :-------- | :----- | :----------------------------------------------------------- |
| arg     | Object | 订阅成功的频道                                               |
| > channel | String | 频道名                                                       |
| > instId  | String | 指数                                                       |
| data    | Array of objects  | 订阅的数据                                                   |
| > instId  | String | 指数，以USD、USDT、BTC 为计价货币的指数，如 `BTC-USDT`       |
| > idxPx   | String | 最新指数价格                                                 |
| > open24h | String | 24小时开盘价                                                 |
| > high24h | String | 24小时指数最高价格                                           |
| > low24h  | String | 24小时指数最低价格                                           |
| > sodUtc0  | String      | UTC 0  时开盘价                                   |
| > sodUtc8     | String   | UTC+8 时开盘价                                     |
| > ts      | String | 指数价格更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
