---
site: hongkong
lang: zh
name: WebSocket
type: guide
---

## WebSocket [trading-account-websocket]
### 账户频道 [trading-account-websocket-account-channel]

获取账户信息，首次订阅按照订阅维度推送数据，此外，当下单、撤单、成交等事件触发时，推送数据以及按照订阅维度定时推送数据<br>

该频道的并发连接受到如下规则限制：[WebSocket 连接限制] (/docs-v5/zh/#overview-websocket-connection-count-limit)

#### 服务地址

/ws/v5/private (需要登录)

> 请求示例：单个

```shell
{
  	"id": "1512",
	"op": "subscribe",
	"args": [{
		"channel": "account",
		"ccy": "BTC"
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
		"channel": "account",
		"ccy": "BTC"
	}]

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
      "channel": "account",
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
          "channel": "account",
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

| 参数      | 类型   | 是否必须 | 描述                                                |
| :-------- | :----- | :------- | :-------------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是       | 操作<br>`subscribe`<br>`unsubscribe` |
| args    | Array of objects  | 是       | 请求订阅的频道列表                                  |
| > channel | String | 是       | 频道名<br>`account`                                   |
| > ccy     | String | 否       | 币种                                                |
| > extraParams | String | 否    | 额外配置                           |
| >> updateInterval     | int | 否    | `0`: 仅根据账户事件推送数据 <br>若不添加该字段或将其设置为除0外的其他值，数据将根据事件推送并定时推送。 <br>使用该字段需严格遵守以下格式。 <br>"extraParams": "<br>{<br> \"updateInterval\": \"0\"<br>}<br>"                                        |

> 成功返回示例：单个

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "account",
		"ccy": "BTC"
	},
  "connId": "a4d3ae55"
}
```

```python
import asyncio

from okx.websocket.WsPrivateAsync import WsPrivateAsync

def privateCallback(message):
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
    args = [{"channel": "account"}]

    await ws.subscribe(args, callback=privateCallback)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=privateCallback)
    await asyncio.sleep(10)

asyncio.run(main())

```

> 成功返回示例

```json
{
  	"id": "1512",
	"event": "subscribe",
	"arg": {
		"channel": "account"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"account\", \"ccy\" : \"BTC\"}]}",
  "connId": "a4d3ae55"
}
```
#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                           |
| :--------- | :----- | :------- |:---------------------------------------------|
| id     | String | 否       | 消息的唯一标识 |
| event      | String | 是       | 事件<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg      | Object | 否       | 订阅的频道                                        |
| > channel | String | 是       | 频道名<br>`account`                                |
| > ccy     | String | 否       | 币种                                           |
| code       | String | 否       | 错误码                                          |
| msg        | String | 否       | 错误消息                                         |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "account",
		"uid": "44*********584"
	},
	"eventType": "snapshot",
	"curPage": 1,
	"lastPage": true,
	"data": [{
		"adjEq": "55444.12216906034",
        "availEq": "55444.12216906034",
		"borrowFroz": "0",
		"delta": "0",
        "deltaLever": "0",
        "deltaNeutralStatus": "0",
		"details": [{
			"availBal": "4734.371190691436",
			"availEq": "4734.371190691435",
			"borrowFroz": "0",
			"cashBal": "4750.426970691436",
			"ccy": "USDT",
			"coinUsdPrice": "0.99927",
			"crossLiab": "0",
			"colRes": "0",
			"collateralEnabled": false,
			"collateralRestrict": false, // 已弃用，请使用colRes
			"colBorrAutoConversion": "0",
			"disEq": "4889.379316336831",
			"eq": "4892.951170691435",
			"eqUsd": "4889.379316336831",
			"smtSyncEq": "0",
			"spotCopyTradingEq": "0",
			"fixedBal": "0",
			"frozenBal": "158.57998",
			"imr": "",
			"interest": "0",
			"isoEq": "0",
			"isoLiab": "0",
			"isoUpl": "0",
			"liab": "0",
			"maxLoan": "0",
			"mgnRatio": "",
			"mmr": "",
			"notionalLever": "",
			"ordFrozen": "0",
			"rewardBal": "0",
			"spotInUseAmt": "",
			"clSpotInUseAmt": "",
			"maxSpotInUseAmt": "",			
			"spotIsoBal": "0",
			"stgyEq": "150",
			"twap": "0",
			"uTime": "1705564213903",
			"upl": "-7.475800000000003",
			"uplLiab": "0",
			"spotBal": "",
			"openAvgPx": "",
			"accAvgPx": "",
			"spotUpl": "",
			"spotUplRatio": "",
			"totalPnl": "",
			"totalPnlRatio": ""
		}],
		"imr": "0",
		"isoEq": "0",
		"mgnRatio": "",
		"mmr": "0",
		"notionalUsd": "0",
        "notionalUsdForBorrow": "0",
        "notionalUsdForFutures": "0",
        "notionalUsdForOption": "0",
        "notionalUsdForSwap": "0",
		"ordFroz": "0",
		"totalEq": "55868.06403501676",
		"uTime": "1705564223311",
		"upl": "0"
	}]
}
```
#### 推送数据参数

| **参数名**                 | **类型**   | **描述**                                                                                        |
|:------------------------|:---------|:----------------------------------------------------------------------------------------------|
| arg                     | Object   | 请求订阅的频道                                                                                       |
| > channel               | String   | 频道名                                                                                           |
| > uid                   | String   | 用户标识                                                                                          |
| eventType               | String   | 事件类型：<br>`snapshot`: 首推及定时快照推送 <br>`event_update`：事件推送 |
| curPage                 | Integer  | 当前消息分页页数 <br>仅适用于`snapshot`事件类型，`event_update`时不返回。 |
| lastPage                | Boolean  | 当前消息是否为最后一页：<br>`true`<br>`false`<br>仅适用于`snapshot`事件类型，`event_update`时不返回. |
| data                    | Array of objects    | 订阅的数据                                                                                         |
| > uTime                 | String   | 获取账户信息的最新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                   |
| > totalEq               | String   | 美金层面权益                                                                                        |
| > isoEq                 | String   | 美金层面逐仓仓位权益<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                               |
| > adjEq                 | String   | 美金层面有效保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| > availEq | String | 账户美金层面可用保证金，排除因总质押借币上限而被限制的币种<br> 适用于`跨币种保证金模式/组合保证金模式` |
| > ordFroz               | String   | 美金层面全仓挂单占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`                                                        |
| > imr                   | String   | 美金层面占用保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| > mmr                   | String   | 美金层面维持保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| > borrowFroz            | String   | 账户美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                               |
| > mgnRatio              | String   | 美金层面维持保证金率<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                    |
| > notionalUsd           | String   | 以美金价值为单位的持仓数量，即仓位美金价值<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                       |
| > notionalUsdForBorrow  | String   | 借币金额（美元价值）<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| > notionalUsdForSwap    | String   | 永续合约持仓美元价值<br>适用于`跨币种保证金模式`/`组合保证金模式`        |
| > notionalUsdForFutures | String   | 交割合约持仓美元价值<br>适用于`跨币种保证金模式`/`组合保证金模式`        |
| > notionalUsdForOption  | String   | 期权持仓美元价值<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                     |
| > upl                   | String   | 账户层面全仓未实现盈亏（美元单位）<br>适用于`跨币种保证金模式`/`组合保证金模式`                                                  |
| > delta | String | Delta (USD) |
| > deltaLever | String | Delta权益比率<br>deltaLever = delta/totalEq |
| > deltaNeutralStatus | String | Delta 风险状态<br>`0`: 普通<br>`1`: 限制划转<br>`2`: 仅支持降低 Delta - 相同基础货币的现货、交割和永续合约视为同一标的资产。同一标的资产内，仅能新下一笔降低 Delta 值的订单，且下单时不应存在其他挂单。如果触发此限制，且您的账户 Delta 大于 500,000 USD，您的所有限价、市价、高级限价单挂单将被撤销。 |
| > details               | Array    | 各币种资产详细信息                                                                                     |
| >> ccy                  | String   | 币种                                                                                            |
| >> eq                   | String   | 币种总权益                                                                                         |
| >> cashBal              | String   | 币种余额                                                                                          |
| >> uTime                | String   | 币种余额信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                   |
| >> isoEq                | String   | 币种逐仓仓位权益<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                                 |
| >> availEq              | String   | 可用保证金<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                                    |
| >> disEq                | String   | 美金层面币种折算权益                                                                                    |
| >> fixedBal             | String   | 抄底宝、逃顶宝功能的币种冻结金额                                                                              |
| >> availBal             | String   | 可用余额                                                                                          |
| >> frozenBal            | String   | 币种占用金额                                                                                        |
| >> ordFrozen            | String   | 挂单冻结数量<br>适用于`现货模式`/`合约模式`/`跨币种保证金模式`                                                      |
| >> liab                 | String   | 币种负债额<br>值为正数，如 `21625.64`<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                  |
| >> upl                  | String   | 未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                                    |
| >> uplLiab              | String   | 由于仓位未实现亏损导致的负债<br>适用于`跨币种保证金模式`/`组合保证金模式`                                                     |
| >> crossLiab            | String   | 币种全仓负债额<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                     |
| >> isoLiab              | String   | 币种逐仓负债额<br>适用于`跨币种保证金模式`/`组合保证金模式`                                                            |
| >> rewardBal            | String   | 体验金余额                                                                                         |
| >> mgnRatio             | String   | 币种全仓维持保证金率，衡量账户内某项资产风险的指标<br>适用于`合约模式`且有全仓仓位时                                                |
| >> imr                  | String   | 币种维度全仓占用保证金<br>适用于`合约模式`且有全仓仓位时                                                            |
| >> mmr                  | String   | 币种维度全仓维持保证金<br>适用于`合约模式`且有全仓仓位时                                                            |
| >> interest             | String   | 计息<br>值为正数，如 `9.01`<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                         |
| >> twap                 | String   | 当前负债币种触发自动换币的风险<br>0、1、2、3、4、5其中之一，数字越大代表您的负债币种触发自动换币概率越高<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| >> frpType | String | 自动换币类型<br> `0`：未发生自动换币<br> `1`：基于用户的自动换币<br> `2`：基于平台借币限额的自动换币<br><br> 当twap>=1时返回1或2代表自动换币风险类型，适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| >> maxLoan              | String   | 币种最大可借<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` 的全仓                                                  |
| >> eqUsd                | String   | 币种权益美金价值                                                                                      |
| >> notionalLever        | String   | 币种杠杆倍数<br>适用于`合约模式`                                                                        |
| >> coinUsdPrice         | String   | 币种美元指数                                                                                        |
| >> stgyEq               | String   | 策略权益                                                                                          |
| >> isoUpl               | String   | 逐仓未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                                  |
| >> borrowFroz           | String   | 币种美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                               |
| >> spotInUseAmt         | String   | 现货对冲占用数量<br>适用于`组合保证金模式`                                                                      |
| >> clSpotInUseAmt       | String   | 用户自定义现货占用数量<br>适用于`组合保证金模式`                                                                   |
| >> maxSpotInUseAmt      | String   | 系统计算得到的最大可能现货占用数量<br>适用于`组合保证金模式`                                                             |
| >> spotIsoBal           | String   | 现货逐仓余额<br>仅适用于现货带单/跟单<br>适用于`现货模式`/`合约模式`                                           |
| >> smtSyncEq            | String   | 合约智能跟单权益<br>默认为0，仅适用于跟单人。                                                                     |
| >> spotCopyTradingEq    | String   | 现货智能跟单权益<br>默认为0，仅适用于跟单人。                                                                     |
| >> spotBal              | String   | 现货余额 ，单位为 币种，比如 BTC。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)         |
| >> openAvgPx            | String   | 现货开仓成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)             |
| >> accAvgPx             | String   | 现货累计成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)             |
| >> spotUpl              | String   | 现货未实现收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)             |
| >> spotUplRatio         | String   | 现货未实现收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)                    |
| >> totalPnl             | String   | 现货累计收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)              |
| >> totalPnlRatio        | String   | 现货累计收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)                     |
| >> colRes               | String          | 平台维度质押限制状态<br> `0`：限制未触发<br> `1`：限制未触发，但该币种接近平台质押上限<br> `2`：限制已触发。该币种不可用作新订单的保证金，这可能会导致下单失败。但它仍会被计入账户有效保证金，保证金率不会收到影响。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| >> colBorrAutoConversion | String 		| 基于平台质押借币限额的自动换币风险指标。分为1-5多个等级，数字越大，触发自动换币的可能性越大。默认值为0，表示当前无风险。5表示该用户正在进行自动换币，4代表该用户即将被进行自动换币，1/2/3表示存在自动换币风险。<br> 适用于`现货模式`/`合约模式`/`跨币种保证金模式`/`组合保证金模式`<br> 当某币种的全平台质押借币量超出平台总上限一定比例时，对于质押该币种且借币量较大的用户，平台将通过自动换币降低质押借币风险。请减少该币种的质押数量或偿还负债，以降低风险。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| >> collateralRestrict | Boolean | ~~平台维度的质押借币限制<br> `true`<br> `false`~~（已弃用，请使用colRes） |
| >> collateralEnabled   | Boolean   | `true`：质押币<br>`false`：非质押币<br>适用于`跨币种保证金模式 |

<aside class="notice"> 
<br> - 账户频道基于事件推送，并进行定时推送
<br> - 账户频道的事件推送并非在事件发生时实时进行，而是按照大约50毫秒的固定时间窗口进行聚合推送。例如，在固定时间窗口内发生多个事件，系统将尽量聚合为一条消息并在固定时间窗口结束时进行推送。在数据量过大的情况下可能拆分为多条消息。
<br> - 无论是否有账户维度的变化，定时推送都会发送更新
</aside> 

<aside class="notice"> 
<br> - 只推用户币种层面资产不为0的账户信息。币种层面资产不为0的定义：eq、availEq、availBal 中任意一个字段不为0，即币种层面资产不为0。如果数据太大无法在单个推送消息中发送，它将被分成多个消息发送。
<br> - 例：按照所有币种订阅且有5个币种的余额或者权益都不为0，首次和定时推全部5个；账户下有一个币种余额或者权益改变，那么账户变更的触发只推这一个。
</aside>
