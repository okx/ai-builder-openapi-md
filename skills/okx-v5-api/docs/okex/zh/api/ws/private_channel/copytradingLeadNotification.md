---
site: okex
lang: zh
name: WS / 带单消息通知频道
type: guide
---

### WS / 带单消息通知频道 [order-book-trading-copy-trading-ws-lead-trading-notification-channel]

带单失败时的消息通知

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "copytrading-lead-notification",
		"instType": "SWAP"
	}]
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "copytrading-lead-notification",
		"instType": "SWAP"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数       | 类型   | 是否必须 | 描述    |
| :--------- | :----- | -------- |:------------------------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op         | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                                                                 |
| args     | Array of objects  | 是       | 请求订阅的频道列表                                                                                                         |
| > channel  | String | 是       | 频道名<br>`copytrading-lead-notification`                                                                                                     |
| > instType | String | 是       | 产品类型<br>`SWAP`：永续合约 |
| > instId   | String | 否       | 产品ID  |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "copytrading-lead-notification",
        "instType": "SWAP"
    },
    "connId": "aa993428"
}
```

> 失败返回示例

```json
{
    "id": "1512",
	"event": "error",
	"code": "60012",
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"copytrading-lead-notification\", \"instType\" : \"FUTURES\"}]}",
    "connId":"a4d3ae55"
}
```
#### 返回参数

| 参数               | 类型   | 是否必须 | 描述                                           |
|:-----------------| :----- | -------- |:---------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event            | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg              | Object | 否       | 订阅的频道                                        |
| > channel        | String | 是       | 频道名                                          |
| > instType       | String | 是       | 产品类型<br>`SWAP`：永续合约                        |
| > instId         | String | 否       | 产品ID                                         |
| code             | String | 否       | 错误码                                          |
| msg              | String | 否       | 错误消息                                         |
| connId   | String   | 是  | WebSocket 连接ID                               |

> 推送示例：

```json
{
    "arg": {
        "channel": "copytrading-lead-notification",
        "instType": "SWAP",
        "uid": "525627088439549953"
    },
    "data": [
        {
            "infoType": "2",
            "instId": "",
            "instType": "SWAP",
            "maxLeadTraderNum": "3",
            "minLeadEq": "",
            "posSide": "",
            "side": "",
            "subPosId": "667695035433385984",
            "uniqueCode": "3AF72F63E3EAD701"
        }
    ]
}
```

#### 推送数据参数

| **参数名**                 | **类型** | **描述**                                                                                               |
|:------------------------|:-------|:-----------------------------------------------------------------------------------------------------|
| arg                     | Object | 订阅成功的频道                                                                                              |
| > channel               | String | 频道名                                                                                                  |
| > uid                   | String | 用户标识                                                                                                 |
| > instType              | String | 产品类型                                                                                                 |
| data                    | Array of objects  | 订阅的数据                                                                                                |
| > instType              | String | 产品类型                                                                                                 |
| > infoType                | String | 消息类型<br>`1`: 带单失败，触发最大仓位限制<br>`2`: 带单失败，触发带单次数限制<br>`3`: 带单失败，交易账户 USDT 低于最小权益 |
| > subPosId                   | String | 带单仓位 ID                                                                                              |
| > uniqueCode                 | String | 交易员唯一标识码                                                                                             |
| > instId               | String | 产品 ID                                                                                                |
| > side                 | String | 订单方向，`buy` `sell`                                                                                    |
| > posSide                | String | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                              |
| > maxLeadTraderNum                    | String | 当前交易员单日最大带单次数                                                                                        |
| > minLeadEq           | String | 带单最小 USDT 权益                                                                                         |
