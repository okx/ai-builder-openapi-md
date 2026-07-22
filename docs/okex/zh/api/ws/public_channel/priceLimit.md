---
site: okex
lang: zh
name: 限价频道
type: guide
---

### 限价频道 [public-data-websocket-price-limit-channel]

获取交易产品的最高买价和最低卖价。限价有变化时，每 200 毫秒推送一次数据，限价没变化时，不推送数据

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "price-limit",
		"instId": "LTC-USD-190628"
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
          "channel": "price-limit",
          "instId": "LTC-USD-190628"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                |
| :-------- | :----- | :------- |:----------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args    | Array of objects  | 是       | 请求订阅的频道列表                         |
| > channel | String | 是       | 频道名<br>`price-limit`                 |
| > instId | String | 是 | 产品ID |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "price-limit",
		"instId": "LTC-USD-190628"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"price-limit\", \"instId\" : \"LTC-USD-190628\"}]}",
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
		"channel": "price-limit",
		"instId": "LTC-USD-190628"
	},
	"data": [{
		"instId": "LTC-USD-190628",
		"buyLmt": "200",
		"sellLmt": "300",
		"ts": "1597026383085",
		"enabled": true
	}]
}
```
#### 推送数据参数

| **参数名** | **类型** | **描述**                                                  |
| :--------- | :------- | :-------------------------------------------------------- |
| arg      | Object   | 订阅成功的频道                                            |
| > channel  | String   | 频道名                                                    |
| > instId   | String   | 产品ID                                                    |
| data     | Array of objects    | 订阅的数据                                                |
| > instType  | String   | 产品类型                                                  |
| > instId | String | 产品ID，如 `BTC-USD-SWAP` |
| > buyLmt | String | 最高买价 <br>当enabled为false时，返回"" |
| > sellLmt | String | 最低卖价 <br>当enabled为false时，返回"" |
| > ts | String | 限价数据更新时间 ，Unix时间戳的毫秒数格式，如 `1597026383085` |
| > enabled | Boolean | 限价是否生效 <br>`true`：限价生效 <br>`false`：限价不生效 |
