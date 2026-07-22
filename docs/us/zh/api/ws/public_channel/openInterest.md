---
site: us
lang: zh
name: 持仓总量频道
type: guide
---

### 持仓总量频道 [public-data-websocket-open-interest-channel]

获取持仓总量，每3s有数据更新推送一次数据

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "open-interest",
		"instId": "LTC-USD-SWAP"
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
          "channel": "open-interest",
          "instId": "LTC-USD-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                             |
| :-------- | :----- | :------- | :------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args    | Array of objects  | 是       | 请求订阅的频道列表               |
| > channel | String | 是       | 频道名<br>`open-interest`          |
| > instId  | String | 是       | 产品ID                           |
> 成功返回示例

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "open-interest",
		"instId": "LTC-USD-SWAP"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"open-interest\", \"instId\" : \"LTC-USD-SWAP\"}]}",
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
        "channel": "open-interest",
        "instId": "BTC-USDT-SWAP"
    },
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "oi": "2216113.01000000309",
            "oiCcy": "22161.1301000000309",
            "oiUsd": "1939251795.54769270396321",
            "ts": "1743041250440"
        }
    ]
}
```
#### 推送数据参数

| **参数名** | **类型** | **描述**                                  |
| ---------- | -------- |-----------------------------------------|
| arg      | Object   | 订阅成功的频道                                 |
| > channel   | String   | 频道名                                     |
| > instId    | String   | 产品ID                                    |
| data     | Array of objects    | 订阅的数据                                   |
| > instType  | String   | 产品类型                                    |
| > instId    | String   | 产品ID，如 `BTC-USDT-SWAP`                  |
| > oi        | String   | 持仓量，按张为单位                               |
| > oiCcy     | String   | 持仓量，按币为单位，如 BTC                         |
| > oiUsd | String | 持仓量（按`USD`折算） |
| > ts        | String   | 数据更新的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
