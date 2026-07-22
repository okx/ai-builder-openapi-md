---
site: us
lang: zh
name: 持仓频道
type: guide
---

### 持仓频道 [trading-account-websocket-positions-channel]

获取持仓信息，首次订阅按照订阅维度推送数据，此外，当下单、撤单等事件触发时，推送数据以及按照订阅维度定时推送数据<br>
该频道的并发连接受到如下规则限制：[WebSocket 连接限制] (/docs-v5/zh/#overview-websocket-connection-count-limit)

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例：单个

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "positions",
		"instType": "FUTURES",
		"instFamily": "BTC-USD"
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
    args = [
        {
          "channel": "positions",
          "instType": "FUTURES",
          "instFamily": "BTC-USD"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

> 请求示例

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [
        {
            "channel": "positions",
            "instType": "ANY",
            "extraParams": "
                {
                    \"updateInterval\": \"0\"
                }
            "
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
        url = "wss://ws.okx.com:8443/ws/v5/private",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "positions",
            "instType": "ANY",
            "extraParams": "{\"updateInterval\": \"0\"}"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())
```

#### 请求参数

| 参数                | 类型              | 是否必须   | 描述                                                                                                |
|:------------------|:----------------|--------|:--------------------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op                | String          | 是      | 操作<br>`subscribe`<br>`unsubscribe`                                                                 |
| args              | Array of objects | 是      | 请求订阅的频道列表                                                                                         |
| > channel         | String          | 是      | 频道名<br>`positions`                                                                                   |
| > instType        | String          | 是      | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约<br>`ANY `：全部 |
| > instFamily      | String          | 否      | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                                          |
| > instId          | String          | 否      | 产品ID<br> 如果同时传了 instId 和 instFamily，instId 将被使用                                                                                              |
| > extraParams     | String          | 否      | 额外配置                           |
| >> updateInterval | int             | 否      | `0`: 仅根据持仓事件推送数据<br>`2000, 3000, 4000`: 根据持仓事件推送，且根据设置的时间间隔定时推送（ms）<br><br>若不添加该字段或将其设置为上述合法值以外的其他值，数据将根据事件推送并大约每 5 秒定期推送一次。<br><br>使用该字段需严格遵守以下格式。<br>"extraParams": "<br>{<br>\"updateInterval\": \"0\"<br>}<br>" |

> 成功返回示例：单个

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "positions",
		"instType": "FUTURES",
		"instFamily": "BTC-USD"
	},
    "connId": "a4d3ae55"
}
```

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "positions",
		"instType": "ANY"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"positions\", \"instType\" : \"FUTURES\"}]}",
    "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数           | 类型       | 是否必须 | 描述                                                                                    |
|:-------------|:---------|------|:--------------------------------------------------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event        | String   | 是    | 事件<br>`subscribe`<br>`unsubscribe`<br>`error`                                         |
| arg          | Object   | 否    | 订阅的频道                                                                                 |
| > channel    | String   | 是    | 频道名                                                                                   |
| > instType   | String   | 是    | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约<br>`ANY`：全部     |
| > instFamily | String   | 否    | 交易品种<br>适用于`交割`/`永续`/`期权`                                                             |
| > instId     | String   | 否    | 产品ID                                                                                  |
| code         | String   | 否    | 错误码                                                                                   |
| msg          | String   | 否    | 错误消息                                                                                  |
| connId       | String   | 是    | WebSocket连接ID                                                                         |

> 推送示例：单个

```json
{
    "arg":{
        "channel":"positions",
        "uid": "77982378738415879",
        "instType":"FUTURES"
    },
    "eventType": "snapshot",
	"curPage": 1,
	"lastPage": true,
    "data":[
        {
            "adl":"1",
            "availPos":"1",
            "avgPx":"2566.31",
            "cTime":"1619507758793",
            "ccy":"ETH",
            "deltaBS":"",
            "deltaPA":"",
            "gammaBS":"",
            "gammaPA":"",
			"hedgedPos": "",
            "imr":"",
            "instId":"ETH-USD-210430",
            "instType":"FUTURES",
            "interest":"0",
            "idxPx":"2566.13",
            "last":"2566.22",
            "lever":"10",
            "liab":"",
            "liabCcy":"",
            "liqPx":"2352.8496681818233",
            "markPx":"2353.849",
            "margin":"0.0003896645377994",
            "mgnMode":"isolated",
            "mgnRatio":"11.731726509588816",
            "mmr":"0.0000311811092368",
            "notionalUsd":"2276.2546609009605",
            "optVal":"",
            "pTime":"1619507761462",
            "pendingCloseOrdLiabVal":"0.1",
            "pos":"1",
            "baseBorrowed": "",
            "baseInterest": "",
            "quoteBorrowed": "",
            "quoteInterest": "",
            "posCcy":"",
            "posId":"307173036051017730",
            "posSide":"long",
            "spotInUseAmt": "",
            "clSpotInUseAmt": "",
            "maxSpotInUseAmt": "",
            "spotInUseCcy": "",
            "bizRefId": "",
            "bizRefType": "",
            "thetaBS":"",
            "thetaPA":"",
            "tradeId":"109844",
            "uTime":"1619507761462",
            "upl":"-0.0000009932766034",
            "uplLastPx":"-0.0000009932766034",
            "uplRatio":"-0.0025490556801078",
            "uplRatioLastPx":"-0.0025490556801078",
            "vegaBS":"",
            "vegaPA":"",
            "realizedPnl":"0.001",
            "pnl":"0.0011",
            "fee":"-0.0001",
            "fundingFee":"0",
            "liqPenalty":"0",
            "nonSettleAvgPx":"",
            "settledPnl":"",
            "closeOrderAlgo":[
                {
                    "algoId":"123",
                    "slTriggerPx":"123",
                    "slTriggerPxType":"mark",
                    "tpTriggerPx":"123",
                    "tpTriggerPxType":"mark",
                    "closeFraction":"0.6"
                },
                {
                    "algoId":"123",
                    "slTriggerPx":"123",
                    "slTriggerPxType":"mark",
                    "tpTriggerPx":"123",
                    "tpTriggerPxType":"mark",
                    "closeFraction":"0.4"
                }
            ]
        }
    ]
}
```

> 推送示例

```json
{
	"arg": {
		"channel": "positions",
		"uid": "77982378738415879",
		"instType": "ANY"
	},
    "eventType": "snapshot",
	"curPage": 1,
	"lastPage": true,
	"data": [{
		"adl": "1",
		"availPos": "1",
		"avgPx": "2566.31",
		"cTime": "1619507758793",
		"ccy": "ETH",
		"deltaBS": "",
		"deltaPA": "",
		"gammaBS": "",
		"gammaPA": "",
		"hedgedPos": "",
		"imr": "",
		"instId": "ETH-USD-210430",
		"instType": "FUTURES",
		"interest": "0",
		"idxPx": "2566.13",
		"last": "2566.22",
		"usdPx": "",
		"bePx": "2353.949",
		"lever": "10",
		"liab": "",
		"liabCcy": "",
		"liqPx": "2352.8496681818233",
		"markPx": "2353.849",
		"margin": "0.0003896645377994",
		"mgnMode": "isolated",
		"mgnRatio": "11.731726509588816",
		"mmr": "0.0000311811092368",
		"notionalUsd": "2276.2546609009605",
		"optVal": "",
		"pendingCloseOrdLiabVal": "0.1",
		"pTime": "1619507761462",
		"pos": "1",
		"baseBorrowed": "",
		"baseInterest": "",
		"quoteBorrowed": "",
		"quoteInterest": "",
		"posCcy": "",
		"posId": "307173036051017730",
		"posSide": "long",
		"spotInUseAmt": "",
		"clSpotInUseAmt": "",
		"maxSpotInUseAmt": "",
		"spotInUseCcy": "",
		"bizRefId": "",
		"bizRefType": "",
		"thetaBS": "",
		"thetaPA": "",
		"tradeId": "109844",
		"uTime": "1619507761462",
		"upl": "-0.0000009932766034",
		"uplLastPx": "-0.0000009932766034",
		"uplRatio": "-0.0025490556801078",
		"uplRatioLastPx": "-0.0025490556801078",
		"vegaBS": "",
		"vegaPA": "",
		"realizedPnl": "0.001",
		"pnl": "0.0011",
		"fee": "-0.0001",
		"fundingFee": "0",
		"liqPenalty": "0",
		"nonSettleAvgPx": "",
		"settledPnl": "",
		"closeOrderAlgo": [{
				"algoId": "123",
				"slTriggerPx": "123",
				"slTriggerPxType": "mark",
				"tpTriggerPx": "123",
				"tpTriggerPxType": "mark",
				"closeFraction": "0.6"
			},
			{
				"algoId": "123",
				"slTriggerPx": "123",
				"slTriggerPxType": "mark",
				"tpTriggerPx": "123",
				"tpTriggerPxType": "mark",
				"closeFraction": "0.4"
			}
		]
	}, {
		"adl": "1",
		"availPos": "1",
		"avgPx": "2566.31",
		"cTime": "1619507758793",
		"ccy": "ETH",
		"deltaBS": "",
		"deltaPA": "",
		"gammaBS": "",
		"gammaPA": "",
		"imr": "",
		"hedgedPos": "",
		"instId": "ETH-USD-SWAP",
		"instType": "SWAP",
		"interest": "0",
		"idxPx": "2566.13",
		"last": "2566.22",
		"usdPx": "",
		"bePx": "2353.949",
		"lever": "10",
		"liab": "",
		"liabCcy": "",
		"liqPx": "2352.8496681818233",
		"markPx": "2353.849",
		"margin": "0.0003896645377994",
		"mgnMode": "isolated",
		"mgnRatio": "11.731726509588816",
		"mmr": "0.0000311811092368",
		"notionalUsd": "2276.2546609009605",
		"optVal": "",
		"pendingCloseOrdLiabVal": "0.1",
		"pTime": "1619507761462",
		"pos": "1",
		"baseBorrowed": "",
		"baseInterest": "",
		"quoteBorrowed": "",
		"quoteInterest": "",
		"posCcy": "",
		"posId": "307173036051017730",
		"posSide": "long",
		"spotInUseAmt": "",
		"clSpotInUseAmt": "",
		"maxSpotInUseAmt": "",
		"spotInUseCcy": "",
		"bizRefId": "",
		"bizRefType": "",
		"thetaBS": "",
		"thetaPA": "",
		"tradeId": "109844",
		"uTime": "1619507761462",
		"upl": "-0.0000009932766034",
		"uplLastPx": "-0.0000009932766034",
		"uplRatio": "-0.0025490556801078",
		"uplRatioLastPx": "-0.0025490556801078",
		"vegaBS": "",
		"vegaPA": "",
		"realizedPnl": "0.001",
		"pnl": "0.0011",
		"fee": "-0.0001",
		"fundingFee": "0",
		"liqPenalty": "0",
		"nonSettleAvgPx": "",
		"settledPnl": "",
		"closeOrderAlgo": [{
				"algoId": "123",
				"slTriggerPx": "123",
				"slTriggerPxType": "mark",
				"tpTriggerPx": "123",
				"tpTriggerPxType": "mark",
				"closeFraction": "0.6"
			},
			{
				"algoId": "123",
				"slTriggerPx": "123",
				"slTriggerPxType": "mark",
				"tpTriggerPx": "123",
				"tpTriggerPxType": "mark",
				"closeFraction": "0.4"
			}
		]
	}]
}
```
#### 推送数据参数

| 参数名                      | 类型 | 描述                                                                                                                                            |
|:-------------------------| :------- |:----------------------------------------------------------------------------------------------------------------------------------------------|
| arg                      | Object   | 订阅成功的频道                                                                                                                                       |
| > channel                | String   | 频道名                                                                                                                                           |
| > uid                    | String    | 用户标识                                                                                                                                          |
| > instType               | String   | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| > instFamily             | String   | 交易品种                                                                                                                                          |
| > instId                 | String   | 产品ID                                                                                                                                          |
| eventType                | String   | 事件类型：<br>`snapshot`: 首推及定时快照推送 <br>`event_update`：事件推送 |
| curPage                  | Integer  | 当前消息分页页数 <br>仅适用于`snapshot`事件类型，`event_update`时不返回。 |
| lastPage                 | Boolean  | 当前消息是否为最后一页：<br>`true`<br>`false`<br>仅适用于`snapshot`事件类型，`event_update`时不返回. |
| data                     | Array of objects    | 订阅的数据                                                                                                                                         |
| > instType               | String | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| > mgnMode                | String | 保证金模式， `cross`：全仓  `isolated`：逐仓                                                                                                              |
| > posId                  | String | 持仓ID                                                                                                                                          |
| > posSide                | String | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式（`交割`/`永续`/`期权`：`pos`为正代表开多，`pos`为负代表开空。`币币杠杆`：`posCcy`为交易货币时，代表开多；`posCcy`为计价货币时，代表开空。） |
| > pos                    | String | 持仓数量，逐仓自主划转模式下，转入保证金后会产生pos为`0`的仓位                                                                                                            |
| > hedgedPos | String | 对冲持仓数量<br>仅在delta 中性策略模式的账户返回stgyType:1，对普通策略模式的账户返回"" |
| > baseBal                | String | ~~交易币余额，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                           |
| > quoteBal               | String | ~~计价币余额 ，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                          |
| > baseBorrowed           | String | ~~交易币已借，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                      |
| > baseInterest           | String | ~~交易币计息，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                      |
| > quoteBorrowed          | String | ~~计价币已借，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                      |
| > quoteInterest          | String | ~~计价币计息，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                      |
| > posCcy                 | String | 持仓数量币种，仅适用于`币币杠杆`                                                                                                                             |
| > availPos               | String | 可平仓数量，适用于 `币币杠杆`,`期权`<br>对于杠杆仓位，平仓时，杠杆还清负债后，余下的部分会视为币币交易，如果想要减少币币交易的数量，可通过"获取最大可用数量"接口获取只减仓的可用数量。 |
| > avgPx                  | String   | 开仓平均价                                                                                                                                         |
| > upl                    | String | 未实现收益（以标记价格计算）                                                                                                                                |
| > uplRatio               | String | 未实现收益率（以标记价格计算                                                                                                                                        |
| > uplLastPx              | String   | 以最新成交价格计算的未实现收益，主要做展示使用，实际值还是 upl                                                                                                             |
| > uplRatioLastPx         | String   | 以最新成交价格计算的未实现收益率                                                                                                                              |
| > instId                 | String | 产品ID，如 `BTC-USDT-SWAP`                                                                                                                         |
| > lever                  | String | 杠杆倍数，不适用于`期权卖方`                                                                                                                               |
| > liqPx                  | String | 预估强平价<br>不适用于`期权`                                                                                                                           |
| > markPx                 | String   | 最新标记价格                                                                                                                                          |
| > imr                    | String | 初始保证金，仅适用于`全仓`                                                                                                                                |
| > margin                 | String | 保证金余额，仅适用于`逐仓`，可增减                                                                                                                            |
| > mgnRatio               | String | 维持保证金率                                                                                                                                          |
| > mmr                    | String | 维持保证金                                                                                                                                         |
| > liab                   | String | 负债额，仅适用于`币币杠杆`                                                                                                                                |
| > liabCcy                | String | 负债币种，仅适用于`币币杠杆`                                                                                                                               |
| > interest               | String | 利息，已经生成未扣利息                                                                                                                                   |
| > tradeId                | String | 最新成交ID                                                                                                                                        |
| > notionalUsd            | String | 以美金价值为单位的持仓数量                                                                                                                                 |
| > optVal                 | String | 期权价值，仅适用于`期权`                                                                                                                                 |
| > pendingCloseOrdLiabVal | String | 逐仓杠杆负债对应平仓挂单的数量 |
| > adl                    | String | 自动减仓信号区，分为6档，从0到5，数字越小代表adl强度越弱<br/>仅适用于`交割/永续/期权`                                                                                                                    |
| > bizRefId               | String | 外部业务id，如 体验券id                                                                                                                             |
| > bizRefType             | String | 外部业务类型                                                                                                                                        |
| > ccy                    | String | 占用保证金的币种                                                                                                                                      |
| > last                   | String | 最新成交价                                                                                                                                         |
| > idxPx                  | String | 最新指数价格                                                                                                                                          |
| > usdPx                  | String | 保证金币种的市场最新美金价格 仅适用于`交割`/`永续`/`期权`                                                                                                                                          |
| > bePx                   | String | 盈亏平衡价                                                                                                                                                                      |
| > deltaBS                | String | 美金本位持仓仓位delta，仅适用于`期权`                                                                                                                        |
| > deltaPA                | String | 币本位持仓仓位delta，仅适用于`期权`                                                                                                                         |
| > gammaBS                | String | 美金本位持仓仓位gamma，仅适用于`期权`                                                                                                                        |
| > gammaPA                | String | 币本位持仓仓位gamma，仅适用于`期权`                                                                                                                         |
| > thetaBS                | String | 美金本位持仓仓位theta，仅适用于`期权`                                                                                                                        |
| > thetaPA                | String | 币本位持仓仓位theta，仅适用于`期权`                                                                                                                         |
| > vegaBS                 | String | 美金本位持仓仓位vega，仅适用于`期权`                                                                                                                         |
| > vegaPA                 | String | 币本位持仓仓位vega，仅适用于`期权`                                                                                                                          |
| > spotInUseAmt           | String | 现货对冲占用数量<br>适用于`组合保证金模式`                                                                                                                      |
| > clSpotInUseAmt         | String | 用户自定义现货占用数量<br>适用于`组合保证金模式` |
| > maxSpotInUseAmt        | String | 系统计算得到的最大可能现货占用数量<br>适用于`组合保证金模式` |
| > spotInUseCcy           | String | 现货对冲占用币种，如 `BTC`<br>适用于`组合保证金模式`                                                                                                              |
| > realizedPnl            | String | 已实现收益<br/>仅适用于`交割`/`永续`/`期权`<br/>realizedPnl=pnl+fee+fundingFee+liqPenalty+settledPnl                      |
| > pnl                    | String | 平仓订单累计收益额(不包括手续费)                  |
| > fee                    | String | 累计手续费金额，正数代表平台返佣 ，负数代表平台扣除 |
| > fundingFee             | String | 累计资金费用                     |
| > liqPenalty             | String | 累计爆仓罚金，有值时为负数。             |
| > closeOrderAlgo         | Array of objects | 平仓策略委托订单。调用策略委托下单，且`closeFraction`=1 时，该数组才会有值。                                                                                               |
| >> algoId                | String | 策略委托单ID                                                                                                                                       |
| >> slTriggerPx           | String | 止损触发价                                                                                                                                         |
| >> slTriggerPxType       | String | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                         |
| >> tpTriggerPx           | String | 止盈委托价                                                                                                                                         |
| >> tpTriggerPxType       | String | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                         |
| >> closeFraction         | String | 策略委托触发时，平仓的百分比。1 代表100%                                                                                                                       |
| > cTime                  | String   | 持仓创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                        |
| > uTime                  | String | 最近一次持仓更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                    |
| > pTime                  | String | 持仓信息的推送时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                     |
| > nonSettleAvgPx         | String    | 未结算均价<br>不受结算影响的加权开仓价格，仅在新增头寸时更新，和开仓均价的主要区别在于是否受到结算影响。<br>适用于`全仓``交割`        |
| > settledPnl             | String    | 累计已结算收益（以结算价格计算）<br>适用于`全仓``交割` |

<aside class="notice"> 
<br> - 持仓频道基于事件推送，并进行定时推送。
<br> - 持仓频道的事件推送并非在事件发生时实时进行，而是按照大约50毫秒的固定时间窗口进行聚合推送。例如，在固定时间窗口内发生多个事件，系统将尽量聚合为一条消息并在固定时间窗口结束时进行推送。在数据量过大的情况下可能拆分为多条消息。
<br> - 无论是否有仓位的变化，定时推送都会发送更新。
<br> - 若事件推送和定时推送同时发生，系统将在发送一次事件推送消息后再发送一次定时推送消息。
</aside>

<aside class="notice">
Portfolio Margin 账户下，持仓的 IMR MMR的数据是后端服务以ristUnit为最小粒度重新计算，相同riskUnit全仓仓位的imr和mmr返回值相同。
</aside>

<aside class="notice">
逐仓交易设置里是自主划转模式，转入保证金后会推送持仓量为0的仓位。
</aside>

<aside class="notice">
<br> - 只推用户持有的仓位。用户持仓仓位定义：持逐仓自主划转模式下的逐仓仓位pos=0，pos>0或者pos<0都认为持有仓位。如果数据太大无法在单个推送消息中发送，它将被分成多个消息发送。
<br> - 例：按underlying订阅且该underlying下有20个持仓，首次和定时推全部20个；持仓下有一个成交改变其中的一个持仓，那么持仓变更只推这一个。
</aside> 

<aside class="notice">
与交割合约不同，期权持仓到期之后，期权持仓在到期后会自动行权或作废，持仓本身随即消失，因此，该频道不会推送期权到期的信息。
</aside>
