---
site: okex
lang: zh
name: 标记价格频道
type: guide
---

### 标记价格频道 [public-data-websocket-mark-price-channel]

获取标记价格，标记价格有变化时，每200ms推送一次数据，标记价格没变化时，每10s推送一次数据

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
  "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "mark-price",
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
    args = [{
		"channel": "mark-price",
		"instId": "BTC-USDT"
	}]

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
| > channel | String | 是       | 频道名<br>`mark-price`                                |
| > instId  | String | 是       | 产品ID                                              |

> 成功返回示例

```json
{
  "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "mark-price",
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"mark-price\", \"instId\" : \"LTC-USD-190628\"}]}",
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
| > instId  | String | 否       | 产品ID                                                       |
| code      | String | 否       | 错误码                                                       |
| msg       | String | 否       | 错误消息                                                     |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
  "arg": {
    "channel": "mark-price",
    "instId": "BTC-USDT"
  },
  "data": [
    {
      "instType": "MARGIN",
      "instId": "BTC-USDT",
      "markPx": "42310.6",
      "ts": "1630049139746"
    }
  ]
}
```
#### 推送数据参数

| 参数名    | 类型   | 描述                                                         |
| :-------- | :----- | :----------------------------------------------------------- |
| arg     | Object | 订阅成功的频道                                               |
| > channel | String | 频道名                                                       |
| > instId  | String | 产品ID                                                       |
| data    | Array of objects  | 订阅的数据                                                   |
| > instType | String | 交易品种                                                     |
| > instId   | String | 产品ID                                                       |
| > markPx   | String | 标记价格                                                     |
| > ts       | String | 标记价格数据更新时间 ，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
在极少数情况下，客户端可能在短时间内收到两条时间戳相同的标记价格消息。这可能发生在系统维护或服务发布期间，且不会持续出现。当出现此情况时，客户端应以后收到的消息作为权威值。两条消息的差值可忽略不计，不会对交易策略产生实质性影响。
</aside>
