---
site: okex
lang: zh
name: 账户greeks频道
type: guide
---

### 账户greeks频道 [trading-account-websocket-account-greeks-channel]

获取账户资产的greeks信息。当增加或者减少币种余额、持仓数量等会触发事件推送，周期性的也会有定时推送。<br>
该频道的并发连接受到如下规则限制：[WebSocket 连接限制] (/docs-v5/zh/#overview-websocket-connection-count-limit)

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例

```shell
{
    "id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "account-greeks"
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
        url = "wss://ws.okx.com:8443/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [{
		"channel": "account-greeks"
	}]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                                                                |
| :-------- | :----- | :------- |:--------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe`                                                                |
| args      | Array of objects  | 是       | 请求订阅的频道列表                                                                                         |
| > channel | String | 是       | 频道名<br>`account-greeks`                                                                           |
| > ccy     | String | 否       | 保证金币种<br>当用户指定了保证金，只有作为保证金的仓位发生变化的时候，才会触发事件推送。例如当指定了ccy = `BTC`，如果 `BTC-USDT-SWAP` 仓位发生变化，不会触发事件推送。 |

> 成功返回示例

```json
{
    "id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "account-greeks"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account-greeks\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                           |
| :--------- | :----- |:-----|:---------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg        | Object | 否    | 订阅的频道                                        |
| > channel  | String | 是    | 频道名<br>`account-greeks`                         |
| > ccy      | String | 否    | 保证金币种                                           |
| code       | String | 否    | 错误码                                          |
| msg        | String | 否    | 错误消息                                         |
| connId   | String   | 是    | WebSocket连接ID |

> 推送示例：单个

```json
{
    "arg": {
        "channel": "account-greeks",
        "ccy": "BTC",
        "uid": "614488474791936"
    },
    "data": [
        {
            "ccy": "BTC",
            "deltaBS": "1.1246665401944310",
            "deltaPA": "-0.0074076183688949",
            "gammaBS": "0.0000000000000000",
            "gammaPA": "0.0148152367377899",
            "thetaBS": "2.0356991946421226",
            "thetaPA": "-0.0000000200174309",
            "ts": "1729179082006",
            "vegaBS": "0.0000000000000000",
            "vegaPA": "0.0000000000000000"
        }
    ]
}
```

> 推送示例

```json
{
    "arg": {
        "channel": "account-greeks",
        "uid": "614488474791936"
    },
    "data": [
        {
            "ccy": "BTC",
            "deltaBS": "1.1246665403011684",
            "deltaPA": "-0.0074021163991037",
            "gammaBS": "0.0000000000000000",
            "gammaPA": "0.0148042327982075",
            "thetaBS": "2.1342098201092528",
            "thetaPA": "-0.0000000200876441",
            "ts": "1729179001692",
            "vegaBS": "0.0000000000000000",
            "vegaPA": "0.0000000000000000"
        },
        {
            "ccy": "ETH",
            "deltaBS": "0.3810670161698570",
            "deltaPA": "-0.0688347042402955",
            "gammaBS": "-0.0000000000230396",
            "gammaPA": "0.1376693483440320",
            "thetaBS": "0.3314776517141782",
            "thetaPA": "0.0000000001316008",
            "ts": "1729179001692",
            "vegaBS": "-0.0000000045069794",
            "vegaPA": "-0.0000000000017267"
        }
    ]
}
```
#### 推送数据参数

| **参数名**      | **类型** | **描述**                                                     |
|:-------------| :------- | :----------------------------------------------------------- |
| arg          | Object   | 请求订阅的频道                                               |
| > channel    | String   | 频道名                                                       |
| > uid        | String   | 用户标识                                                     |
| data         | Array of objects    | 订阅的数据                                                   |
| > deltaBS    | String   | 美金本位账户资产delta     |
| > deltaPA    | String   | 币本位账户资产delta                                                 |
| > gammaBS    | String   | 美金本位账户资产gamma，仅适用于期权全仓 |
| > gammaPA    | String   | 币本位账户资产gamma，仅适用于期权全仓        |
| > thetaBS    | String   | 美金本位账户资产theta，仅适用于期权全仓          |
| > thetaPA    | String   | 币本位账户资产theta，仅适用于期权全仓          |
| > vegaBS     | String   | 美金本位账户资产vega，仅适用于期权全仓           |
| > vegaPA     | String   | 币本位账户资产vega，仅适用于期权全仓            |
| > ccy        | String   | 币种  |
| > ts         | String   | 获取greeks的时间，Unix时间戳的毫秒数格式，如 1597026383085     |

<aside class="notice"> 
账户greeks频道基于事件推送，并进行定时推送<br>
- greeks频道的事件推送并非在事件发生时实时进行，而是按照大约50毫秒的固定时间窗口进行聚合推送。例如，在固定时间窗口内发生多个事件，系统将尽量聚合为一条消息并在固定时间窗口结束时进行推送。在数据量过大的情况下可能拆分为多条消息。
<br> 
- 当用户订阅时指定了保证金币种(ccy)，只有作为保证金的仓位发生变化的时候，才会触发事件推送。例如订阅时指定了ccy=`BTC`，如果`BTC-USDT-SWAP`仓位发生变化，不会触发事件推送。
<br>- 无论是否有greeks数据的变化，定时推送都会发送更新。
</aside> 

<aside class="notice"> 
<br> - 只推账户资产不为0的greeks数据。如果数据太大无法在单个推送消息中发送，它将被分成多个消息发送。
<br> - 例：按照所有币种订阅且有5个币种资产都不为0，首次和定时推全部5个；账户的某个币种资产改变，那么账户greeks变更的触发只推这一个。
</aside>
