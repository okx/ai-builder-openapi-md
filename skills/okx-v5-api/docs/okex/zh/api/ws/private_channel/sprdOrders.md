---
site: okex
lang: zh
name: WebSocket私有频道
type: guide
---

## WebSocket私有频道 [spread-trading-websocket-private-channel]

- 实盘地址: `wss://ws.okx.com/ws/v5/business`
- 模拟盘地址: `wss://wspap.okx.com/ws/v5/business`

### 订单频道 [spread-trading-websocket-private-channel-order-channel]

通过订阅`sprd-orders`频道获取Spread订单信息，首次订阅不推送，只有当下单、撤单等事件触发时，推送数据。

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例：单个
> 

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-orders",
      "sprdId": "BTC-USDT_BTC-USDT-SWAP"
    }
  ]
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
    args = [
        {
          "channel": "sprd-orders",
          "sprdId": "BTC-USDT_BTC-USDT-SWAP"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> 请求示例：
> 

```shell
{
  "id": "1512",
  "op": "subscribe",
  "args": [
    {
      "channel": "sprd-orders",
    }
  ]
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
    args = [
        {
          "channel": "sprd-orders",
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op | String | 是 | 操作<br>`subscribe`<br>`unsubscribe` |
| args | Array of objects | 是 | 请求订阅的频道列表 |
| > channel | String | 是 | 频道名<br>`sprd-orders` |
| > sprdId | String | 是 | Spread ID |

> 成功返回示例：单个
> 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-orders",
    "sprdId": "BTC-USDT_BTC-UST-SWAP"
  },
  "connId": "a4d3ae55"
}

```

> 成功返回示例
> 

```json
{
  "id": "1512",
  "event": "subscribe",
  "arg": {
    "channel": "sprd-orders"
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
  "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"sprd-orders\", \"instType\" : \"FUTURES\"}]}",
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
| > sprdId | String | 否 | Spread ID |
| code | String | 否 | 错误码 |
| msg | String | 否 | 错误消息 |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例：单个
> 

```json
{
  "arg": {
        "channel": "sprd-orders",
        "sprdId": "BTC-USDT_BTC-USDT-SWAP",
        "uid": "614488474791936"
    },
  "data": [
     {
      "sprdId": "BTC-USDT_BTC-UST-SWAP",
      "ordId": "312269865356374016",
      "clOrdId": "b1",
      "tag": "",
      "px": "999",
      "sz": "3",
      "ordType": "limit",
      "side": "buy",
      "fillSz": "0",
      "fillPx": "",
      "tradeId": "",
      "accFillSz": "0",
      "pendingFillSz": "2",
      "pendingSettleSz": "1",
      "canceledSz": "1",
      "state": "live",
      "avgPx": "0",
      "cancelSource": "",
      "uTime": "1597026383085",
      "cTime": "1597026383085",
      "code": "0",
      "msg": "",
      "reqId": "",
      "amendResult": ""
    }
  ]

}

```

#### 推送数据参数

| 参数名               | 类型 | 描述                                                                                                                                                                                                                               |
|-------------------| --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg               | Object | 订阅成功的频道                                                                                                                                                                                                                          |
| > channel         | String | 频道名                                                                                                                                                                                                                              |
| > uid             | String | 用户标识                                                                                                                                                                                                                             |
| > sprdId          | String | spread ID                                                                                                                                                                                                                        |
| data              | Array of objects | 订阅的数据                                                                                                                                                                                                                            |
| > sprdId          | String | spread ID                                                                                                                                                                                                                        |
| > ordId           | String | 订单ID                                                                                                                                                                                                                             |
| > clOrdId         | String | 由用户设置的订单ID来识别您的订单                                                                                                                                                                                                                |
| > tag             | String | 订单标签                                                                                                                                                                                                                             |
| > px              | String | 委托价格                                                                                                                                                                                                                             |
| > sz              | String | 原始委托数量，单位szCcy                                                                                                                                                                                                                   |
| > ordType         | String | 订单类型<br>`market`：市价单 <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余                                                                                                                                                |
| > side            | String | 订单方向 <br>`buy` <br>`sell`                                                                                                                                                                                                        |
| > fillSz          | String | 最新成交数量，适用于结算成功的订单更新                                                                                                                                                                                                              |
| > fillPx          | String | 最新成交价格，适用于结算成功的订单更新                                                                                                                                                                                                              |
| > tradeId         | String | 最近成交ID                                                                                                                                                                                                                           |
| > accFillSz       | String | 累计成交数量                                                                                                                                                                                                                           |
| > pendingFillSz   | String | 待成交数量，包括待结算数量                                                                                                                                                                                                                    |
| > pendingSettleSz | String | 待结算数量                                                                                                                                                                                                                            |
| > canceledSz      | String | 撤单数量                                                                                                                                                                                                                             |
| > avgPx           | String | 成交均价，如果成交数量为0，该字段为"0"                                                                                                                                                                                                            |
| > state           | String | 订单状态<br>`canceled`：撤单成功<br>`live`：等待成交<br>`partially_filled`：部分成交<br>`filled`：完全成交                                                                                                                                               |
| > cancelSource    | String | 撤单原因<br>`0`: 系统撤单<br>`1`: 用户撤单 <br>`14`: 已撤单：IOC 委托订单未完全成交，仅部分成交，导致部分挂单被撤回<br>`15`: 已撤单：该订单委托价不在限价范围内<br> `20`: 系统倒计时撤单<br>`31`: 当前只挂单订单 (Post only) 将会吃掉挂单深度<br> `32`: 自成交保护<br>`34`: 订单结算失败因为保证金不足 <br>`35`: 撤单因为其他订单保证金不足<br>`44`：由于该币种的可用余额不足，无法在触发自动换币后进行兑换，您的订单已撤销，撤销订单后恢复的余额将用于自动换币。当该币种的总抵押借贷量达到平台抵押借贷风控上限时，则会触发自动换币。 |
| > uTime           | String | 订单更新时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                                                                                                             |
| > cTime           | String | 订单创建时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                                                                                                             |
| > code            | String | 错误码，默认为0                                                                                                                                                                                                                         |
| > msg             | String | 错误消息，默认为""                                                                                                                                                                                                                       |
| > reqId           | String | 修改订单时使用的request ID，如果没有修改，该字段为""                                                                                                                                                                                                 |
| > amendResult     | String | 修改订单的结果<br>`-1`：失败<br>`0`：成功<br>如果没有修改，该字段为""                                                                                                                                                                                    |
