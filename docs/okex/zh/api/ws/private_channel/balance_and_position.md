---
site: okex
lang: zh
name: 账户余额和持仓频道
type: guide
---

### 账户余额和持仓频道 [trading-account-websocket-balance-and-position-channel]

获取账户余额和持仓信息，首次订阅按照订阅维度推送数据，此外，当成交、资金划转等事件触发时，推送数据。<br>

该频道适用于尽快获取账户现金余额和仓位资产变化的信息。<br>
该频道的并发连接受到如下规则限制：[WebSocket 连接限制] (/docs-v5/zh/#overview-websocket-connection-count-limit)

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "balance_and_position"
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
		"channel": "balance_and_position"
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
| > channel | String | 是       | 频道名<br>`balance_and_position`                                   |

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "balance_and_position"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"balance_and_position\"}]}",
	"connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                           |
| :--------- | :----- | :------- |:---------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg      | Object | 否       | 订阅的频道                                        |
| > channel | String | 是       | 频道名<br>`balance_and_position`                   |
| code       | String | 否       | 错误码                                          |
| msg        | String | 否       | 错误消息                                         |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "balance_and_position",
		"uid": "77982378738415879"
	},
	"data": [{
		"pTime": "1597026383085",
		"eventType": "snapshot",
		"balData": [{
			"ccy": "BTC",
			"cashBal": "1",
            "uTime": "1597026383085"
		}],
		"posData": [{
			"posId": "1111111111",
			"tradeId": "2",
			"instId": "BTC-USD-191018",
			"instType": "FUTURES",
			"mgnMode": "cross",
			"posSide": "long",
			"pos": "10",
			"ccy": "BTC",
			"posCcy": "",
			"avgPx": "3320",
			"nonSettleAvgPx": "",
			"settledPnl": "",
            "uTime": "1597026383085"
		}],
		"trades": [{
			"instId": "BTC-USD-191018",
			"tradeId": "2",
		}]
	}]
}
```

#### 推送数据参数

| **参数名**           | **类型** | **描述**                                                                                                                                                                                                                                                                       |
|:------------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg               | Object   | 请求订阅的频道                                                                                                                                                                                                                                                                      |
| > channel         | String   | 频道名                                                                                                                                                                                                                                                                          |
| > uid             | String   | 用户标识                                                                                                                                                                                                                                                                         |
| data              | Array of objects    | 订阅的数据                                                                                                                                                                                                                                                                        |
| > pTime           | String   | 推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                         |
| > eventType       | String   | 事件类型<br>`snapshot`：首推快照<br>`delivered`：交割<br>`exercised`：行权<br>`transferred`：划转<br>`filled`：成交<br>`liquidation`：强平<br>`claw_back`：穿仓补偿<br>`adl`：ADL自动减仓<br>`funding_fee`：资金费<br>`adjust_margin`：调整保证金<br>`set_leverage`：设置杠杆<br>`interest_deduction`：扣息<br>`settlement`：交割结算 |
| > balData         | String   | 余额数据                                                                                                                                                                                                                                                                         |
| >> ccy            | String   | 币种                                                                                                                                                                                                                                                                           |
| >> cashBal        | String   | 币种余额                                                                                                                                                                                                                                                                         |
| >> uTime          | String   | 币种余额信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                  |
| > posData         | String   | 持仓数据                                                                                                                                                                                                                                                                         |
| >> posId          | String   | 持仓ID                                                                                                                                                                                                                                                                         |
| >> tradeId        | String   | 最新成交ID                                                                                                                                                                                                                                                                       |
| >> instId         | String   | 交易产品ID，如 `BTC-USD-180213`                                                                                                                                                                                                                                                    |
| >> instType       | String   | 交易产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权                                                                                                                                                                                                      |
| >> mgnMode        | String   | 保证金模式<br>`isolated`, `cross`                                                                                                                                                                                                                                                 |
| >> avgPx          | String   | 开仓平均价                                                                                                                                                                                                                                                                        |
| >> ccy            | String   | 占用保证金的币种                                                                                                                                                                                                                                                                     |
| >> posSide        | String   | 持仓方向<br>`long`，`short`，`net`                                                                                                                                                                                                                                                 |
| >> pos            | String   | 持仓数量，逐仓自主划转模式下，转入保证金后会产生pos为`0`的仓位                                                                                                                                                                                                                                           |
| >> baseBal        | String | ~~交易币余额<br>适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                                                                                                                       |
| >> quoteBal       | String | ~~计价币余额<br>适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                                                                                                                       |
| >> posCcy         | String   | 持仓数量币种<br>只适用于币币杠杆仓位。当是交割、永续、期权持仓时，该字段返回“”                                                                                                                                                                                                                                   |
| >> nonSettleAvgPx | String    | 未结算均价<br>不受结算影响的加权开仓价格，仅在新增头寸时更新，和开仓均价的主要区别在于是否受到结算影响。<br>适用于`全仓``交割`        |
| >> settledPnl     | String    | 累计已结算收益（以结算价格计算）<br>适用于`全仓``交割` |
| >> uTime    | String   | 仓位信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                                                                                                                    |
| > trades    | Array of objects | 成交数据                                                                                                                                                                                                                                                                         |
| >> instId   | String | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                                            |
| >> tradeId  | String | 最新成交ID                                                                                                                                                                                                                                                                       |

<aside class="notice"> 
只有账户余额变化，只推balData；只有持仓余额发生变化，只推posData。
</aside> 

<aside class="notice"> 
<br> - 首次推送，只推用户持有的仓位和币种余额不为0的信息。如果数据太大无法在单个推送消息中发送，它将被分成多个消息发送。
<br> - 例：比如按照所有币种订阅且用户有5个币种余额不为0 和20个仓位，那么首推全部5个币种余额列表和20个持仓信息列表；某个订单成交后，那么只推一个币种余额和对应的持仓信息。
</aside>
