---
site: okex
lang: zh
name: POST / 策略委托下单
method: POST
path: /api/v5/trade/order-algo
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 策略委托下单 [order-book-trading-algo-trading-post-place-algo-order]

提供单向止盈止损委托、双向止盈止损委托、追逐限价委托、计划委托、时间加权委托、移动止盈止损委托

#### 限速：20次/2s
#### 跟单交易带单员带单产品的限速：1次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

#### HTTP请求

`POST /api/v5/trade/order-algo`

> 请求示例

```shell
# 止盈止损策略下单
POST /api/v5/trade/order-algo
body
{
    "instId":"BTC-USDT",
    "tdMode":"cross",
    "side":"buy",
    "ordType":"conditional",
    "sz":"2",
    "tpTriggerPx":"15",
    "tpOrdPx":"18"
}

# 计划委托策略下单
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"side": "buy",
	"tdMode": "cross",
	"posSide": "net",
	"sz": "1",
	"ordType": "trigger",
	"triggerPx": "25920",
	"triggerPxType": "last",
	"orderPx": "-1",
	"attachAlgoOrds": [{
		"attachAlgoClOrdId": "",
		"slTriggerPx": "100",
		"slOrdPx": "600",
		"tpTriggerPx": "25921",
		"tpOrdPx": "2001"
	}]
}

# 移动止盈止损策略下单
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"tdMode": "cross",
	"side": "buy",
	"ordType": "move_order_stop",
	"sz": "10",
	"posSide": "net",
	"callbackRatio": "0.05",
	"reduceOnly": true
}

# 时间加权策略下单
POST /api/v5/trade/order-algo
body
{
	"instId": "BTC-USDT-SWAP",
	"tdMode": "cross",
	"side": "buy",
	"ordType": "twap",
	"sz": "10",
	"posSide": "net",
	"szLimit": "10",
	"pxLimit": "100",
	"timeInterval": "10",
	"pxSpread": "10"
}

# 冰山委托策略下单
POST /api/v5/trade/order-algo
body
{
    "instId": "BTC-USDT",
    "tdMode": "cash",
    "side": "buy",
    "ordType": "smart_iceberg",
    "sz": "1000",
    "szLimit": "50",
    "lmtOrderNumber": "5",
    "aggressiveness": "conservative",
    "pxLimit": "95000",
    "side": "buy",
    "posSide": "",
    "ordType": "smart_iceberg",
    "triggerParams": [
      {
          "triggerAction":"start",
          "triggerStrategy":"rsi",
          "timeframe":"30m",
          "thold":"10",
          "triggerCond":"cross",
          "timePeriod":"14"
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 单向止盈止损
result = tradeAPI.place_algo_order(
    instId="BTC-USDT",
    tdMode="cross",
    side="buy",
    ordType="conditional",
    sz="2",
    tpTriggerPx="15",
    tpOrdPx="18"
)
print(result)
```

#### 请求参数

| 参数名        | 类型      | 是否必须 | 描述                                                                                                                                                                                                                                                                                             |
|:-----------|:--------|:-----|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId     | String  | 是    | 产品ID，如 `BTC-USDT`                                                                                                                                                                                                                                                                              |
| tdMode     | String  | 是    | 交易模式<br>保证金模式 `isolated`：逐仓，`cross：`全仓<br>非保证金模式 `cash`：非保证金<br>`spot_isolated`：现货逐仓(仅适用于现货带单)<br>注意：`isolated` 在跨币种保证金模式和组合保证金模式下不可用。                                                                                                                                                                                                 |
| ccy        | String  | 否    | 保证金币种<br>适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                                                                                                                                                                                                                                                 |
| side       | String  | 是    | 订单方向<br>`buy`：买<br>`sell`：卖                                                                                                                                                                                                                                                                    |
| posSide    | String  | 可选   | 持仓方向<br>在开平仓模式下必填，且仅可选择 `long` 或 `short`                                                                                                                                                                                                                                                       |
| ordType    | String  | 是    | 订单类型<br>`conditional`：单向止盈止损<br>`oco`：双向止盈止损<br><br/>`chase`: 追逐限价委托，仅适用于交割和永续<br/>`trigger`：计划委托<br>`move_order_stop`：移动止盈止损<br>`twap`：时间加权委托<br>`smart_iceberg`：冰山委托                                                                                                                                                 |
| sz         | String  | 可选   | 委托数量<br>`sz`和`closeFraction`必填且只能填其一                                                                                                                                                                                                                                                           |
| tag        | String  | 否    | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间                                                                                                                                                                                                                                                 |
| tgtCcy     | String  | 否    | 委托数量的类型<br>`base_ccy`: 交易货币 ；`quote_ccy`：计价货币<br>仅适用于`币币`单向止盈止损市价买单<br>默认买为`计价货币`，卖为`交易货币`                                                                                                                                                                                                     |
| algoClOrdId    | String | 否    | 客户自定义策略订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                                         |
| closeFraction  | String  | 可选   | 策略委托触发时，平仓的百分比。1 代表100%<br>现在系统只支持全部平仓，唯一接受参数为`1`<br>对于同一个仓位，仅支持一笔全部平仓的止盈止损挂单<br><br>仅适用于`交割`或`永续`<br>当`posSide` = `net`时，`reduceOnly`必须为`true`<br>仅适用于止盈止损 `ordType` = `conditional` 或 `oco`<br>仅适用于止盈止损市价订单<br>不支持组合保证金模式<br>`sz`和`closeFraction`必填且只能填其一                                    |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |

<b>止盈止损</b>

<aside class="notice">
用户可预先设置触发价和委托价，等市场价到达触发价时，系统会按委托价自动下单。<br>单向止盈止损可设置单边的止盈或止损；双向止盈止损可设置双边，一边触发后另一边失效。<br>该委托不会预先占用仓位或保证金。
</aside>

了解更多 [止盈止损](/cn/help/11015447687437)

| 参数名      | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                             |
| :---------- | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| tpTriggerPx | String | 否       | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                                                                                                                                                                                      |
| tpTriggerPxType | String | 否   | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                             |
| tpOrdPx         | String | 否       | 止盈委托价<br>对于条件止盈单，如果填写此参数，必须填写`止盈触发价`<br>对于限价止盈单，需填写此参数，不需要填写`止盈触发价` <br>委托价格为-1时，执行市价止盈                                                                                                                                                                                                                  |
| tpOrdKind       | String | 否       | 止盈订单类型<br/>`condition`: 条件单<br/>`limit`: 限价单<br/> 默认为`condition`                                                                                                                                                                                               |
| slTriggerPx     | String | 否       | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                                                                                                                                                                                      |
| slTriggerPxType | String | 否   | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                                                                                                                                                                             |
| slOrdPx     | String | 否       | 止损委托价，如果填写此参数，必须填写`止损触发价` <br>委托价格为`-1`时，执行市价止损                                                                                                                                                                                                                |
| cxlOnClosePos | Boolean | 否 | 决定用户所下的止盈止损订单是否与该交易产品对应的仓位关联。若关联，仓位被全平时，该止盈止损订单会被同时撤销；若不关联，仓位被撤销时，该止盈止损订单不受影响。 <br><br>有效值： <br>`true`：下单与仓位关联的止盈止损订单 <br>`false`：下单与仓位不关联的止盈止损订单 <br><br>默认值为`false`。若传入`true`，用户必须同时传入 reduceOnly = true，说明当下单与仓位关联的止盈止损订单时，必须为只减仓。<br>适用于`合约模式`/`跨币种保证金模式`。 |
| reduceOnly | Boolean | 否    | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                                                                                                        |

<aside class="notice">
止盈止损<br>
当用户进行单向止盈止损委托（ordType=conditional）时，如果用户同时传了止盈止损四个参数，只进行止损的功能校验，忽略止盈的业务逻辑校验。
</aside>

<b>追逐限价委托</b>

<aside class="notice">
追逐限价委托会立即下 Post Only 订单（只做maker单）并跟随深度变动进行改单。<br/>
追逐限价委托和对应的 Post Only 订单不支持改单。
</aside>

| 参数名         | 类型   | 是否必须 | 描述                                                                                                                                                                              |
|:--------------------| :----- |:-----|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| chaseType           | String | 否    | 追逐类型。<br/>`distance`: 买一/卖一价的距离，默认值。<br/>`ratio`: 比例。                                                                                                                           |
| chaseVal             | String | 否    | 追逐值。<br/>当`chaseType`为`distance`时，是到买一/卖一价的距离。<br/>对于 USDT 本位合约，单位为 USDT；<br/>对于 USDC 合约，单位为 USDC；<br/>对于币本位合约，单位为 USD 。<br/>当`chaseType`为`ratio`时，为比率，0.1 代表 10%。<br/> 默认值为 0。 |
| maxChaseType       | String | 可选   | 最大追逐值的类型。<br/>`distance`: 买一/卖一价的距离<br>`ratio`: 比例。0.1 代表 10%。<br/><br/> maxChaseTyep 和 maxChaseVal 需要同时填写或者不填写。                                                                     |
| maxChaseVal        | String | 可选   | 最大追逐值。<br/>当`chaseType`为`distance`时，是到买一/卖一价的的最大距离<br/>当`chaseType`为`ratio`时，指的比率，0.1 代表 10%。                                                                                   |
| reduceOnly | Boolean | 否    | 是否只减仓，`true` 或 `false`，默认`false`<br>仅适用于`币币杠杆`，以及买卖模式下的`交割/永续`<br>仅适用于`合约模式`和`跨币种保证金模式`                                                                                      |

<b>计划委托</b>

<aside class="notice">
当市场价格到达触发价格时，系统将按预先设置的委托价格和数量自动下单。<br>该委托不会预先占用仓位或保证金。<br>
仅适用于币币、交割和永续。
</aside>
了解更多 [计划委托](/cn/help/11015447687437)

| 参数名                 | 类型              | 是否必须  | 描述                                                                                                   |
|:--------------------|:----------------|:------|:-----------------------------------------------------------------------------------------------------|
| triggerPx           | String          | 是     | 触发该策略订单的价格阈值，单位与该产品的 `px` 相同。具体使用哪种价格源取决于 `triggerPxType`（默认为最新成交价）。方向：做空止损单的触发价须低于 orderPx；做多止损单的触发价须高于 orderPx。方向违规将返回错误码 51046–51049。                                                                                                                                                                                                                                                                                                                                                                                               |
| orderPx             | String          | 条件必填     | 触发后提交的委托价格，与 `triggerPx`（决定何时激活）相互独立。设为 `-1` 表示触发后以市价委托；设置具体价格表示触发后以限价委托。当 `advanceOrdType` 为 `chase` 时不适用（追逐委托无固定价格）。
| advanceOrdType      | String | 否 | 计划委托的子订单类型。<br>`fok`：全部成交或立即取消<br>`ioc`：立即成交并取消剩余<br>`chase`：追逐限价委托。仅适用于 FUTURES 和 SWAP。<br>默认为空（按 `orderPx` 下发限价或市价单）。                                          |
| advChaseParams      | Array of objects | 条件必填 | 追逐参数。当 `advanceOrdType` 为 `chase` 时必填。 |
| > chaseType         | String | 条件必填 | 追逐距离单位。<br>`distance`（默认）：与买一价/卖一价的绝对价格距离，以结算货币计。<br>`ratio`：百分比。 |
| > chaseVal          | String | 条件必填 | 追逐值。当 `chaseType` 为 `distance` 时，为与买一价/卖一价的距离（以结算货币计）；当 `ratio` 时，`0.1` 表示 10%。<br>默认值 `0` 表示直接跟随买一价/卖一价；大于 `0` 表示设置一个距离。 |
| > maxChaseType      | String | 条件必填 | 最大追逐距离单位。`distance` 或 `ratio`。须与 `maxChaseVal` 成对出现。 |
| > maxChaseVal       | String | 条件必填 | 最大追逐距离值。须为正数。须与 `maxChaseType` 成对出现。当偏离达到该值时，追逐委托自动撤单。 |
| triggerPxType       | String          | 否     | 触发价格类型：<br>`last`：任意成交价达到或超过 `triggerPx` 时触发——响应最快，但在流动性较差市场中易受短暂插针影响。<br>`index`：基于多交易所合成指数触发——稳定，不受OKX自身插针影响。<br>`mark`：基于OKX标记价格触发——经过平滑处理，抗插针能力强；衍生品推荐使用。<br>现货产品仅支持 `last`。默认为 `last`。                                |
| attachAlgoOrds      | Array of objects | 否     | 附带止盈止损信息<br>适用于`合约模式/跨币种保证金模式/组合保证金模式`<br>当 `advanceOrdType` 为 `chase` 时不适用。                                                           |
| > attachAlgoClOrdId | String          | 否     | 下单附带止盈止损时，客户自定义的策略订单ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下止盈止损委托单时，该值会传给algoClOrdId。 |
| > tpTriggerPx       | String          | 否     | 止盈触发价，如果填写此参数，必须填写`止盈委托价`                                                                            |
| > tpTriggerRatio    | String          | 否     | 止盈触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约 <br> `tpTriggerPx` 和 `tpTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须大于 0，如果主单为卖出订单，必须处于 -1 和 0 之间。                                                                            |
| > tpTriggerPxType   | String          | 否     | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                   |
| > tpOrdPx           | String          | 否     | 止盈委托价，如果填写此参数，必须填写`止盈触发价`<br>委托价格为`-1`时，执行市价止盈                                                       |
| > slTriggerPx       | String          | 否     | 止损触发价，如果填写此参数，必须填写`止损委托价`                                                                            |
| > slTriggerRatio    | String          | 否     | 止损触发比例，0.3 代表 30% <br> 仅适用于`交割`/`永续`合约 <br> `slTriggerPx` 和 `slTriggerRatio` 只能传入其中一个<br> 如果主单为买入订单，必须处于 0 和 1 之间，如果主单为卖出订单，必须大于 0。0 代表删除止损。                                                                            |
| > slTriggerPxType   | String          | 否     | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格<br>默认为`last`                                   |
| > slOrdPx           | String          | 否     | 止损委托价，如果填写此参数，必须填写`止损触发价`<br>委托价格为`-1`时，执行市价止损                                                       |
| > callbackRatio     | String | 可选 | 回调幅度的比例，如 `0.05` 代表 5%。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > callbackSpread    | String | 可选 | 回调幅度的价距。<br>`callbackRatio` 和 `callbackSpread` 必须传入其中一个，且只能传入一个。<br>仅适用于 `ordType` = `move_order_stop` |
| > activePx          | String | 否          | 激活价格。<br>激活价格是移动止盈止损的激活条件，当市场最新成交价达到或超过激活价格，委托被激活。激活后系统开始计算止盈止损的实际触发价格。如果不填写激活价格，即下单后就被激活。<br>仅适用于 `ordType` = `move_order_stop` |

<b>移动止盈止损</b>

<aside class="notice">
移动止盈止损是一种跟踪市场价格的止盈止损，它的触发价格会跟随市场波动而变化，触发成功后会下市价单。<br>
实际触发价格的计算：卖出或开空时，实际触发价格 = 下单成功后最高价-回调幅度 (价距)，或下单成功后最高价 *(1-回调幅度 %) (比例)；买入或开多，实际触发价格 = 下单成功后最低价 + 回调幅度，或下单成功后最低价 *(1+ 回调幅度 %)。同时，您可以利用激活价格来设置委托被激活的价格。
</aside>
了解更多 [移动止盈止损](/cn/help/11015447687437)

| 参数名    | 类型   | 是否必须 | 描述                                                                                                                   |
| :-------- | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------|
| callbackRatio | String | 可选     | 回调幅度的比例，如 "0.05"代表"5%"<br>`callbackRatio`和`callbackSpread`只能传入一个 |
| callbackSpread  | String | 可选     | 回调幅度的价距                                                          |
| activePx  | String | 否       | 激活价格<br>激活价格是移动止盈止损的激活条件，当市场最新成交价达到或超过激活价格，委托被激活。激活后系统开始计算止盈止损的实际触发价格。如果不填写激活价格，即下单后就被激活。                            |
| reduceOnly | Boolean | 否    | 是否只减仓，`true` 或 `false`，默认`false`<br>该参数仅在 `交割/永续` 的买卖模式下有效，开平模式忽略此参数                                                 |

<b>时间加权</b>

<aside class="notice">
时间加权是一种大额订单拆分后分时吃单的策略。<br>
用户在进行大额交易时，为避免对市场造成过大冲击，需要将大单委托自动拆为多笔委托。
</aside>

了解更多 [时间加权委托](/cn/help/xiii-time-weighted-average-price-twap)

| 参数名    | 类型   | 是否必须 | 描述                                                                                                                                          |
| :-------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------|
| pxVar     | String | 可选    | 吃单价优于盘口的比例，取值范围在 [0.0001,0.01] 之间，如 "0.01"代表"1%"<br>以买入为例，市价低于限制价时，策略开始用买一价向上取一定比例的委托价来委托小额买单。当前这个参数就用来确定向上的比例。<br>`pxVar`和`pxSpread`只能传入一个 |
| pxSpread  | String | 可选    | 吃单单价优于盘口的价距，取值范围不小于0（无上限）<br>以买入为例，市价低于限制价时，策略开始用买一价向上取一定价距的委托价来委托小额买单。当前这个参数就用来确定向上的价距。                                                    |
| szLimit   | String | 是       | 单笔数量<br>以买入为例，市价低于 “限制价” 时，策略开始用买一价向上取一定价距 / 比例的委托价来委托 “一定数量” 的买单。当前这个参数用来确定其中的 “一定数量”。                                                     |
| pxLimit   | String | 是       | 吃单限制价，取值范围不小于0（无上限）<br>以买入为例，市价低于 “限制价” 时，策略开始用买一价向上取一定价距 / 比例的委托价来委托小额买单。当前这个参数就是其中的 “限制价”。                                                                                                                                   |
| timeInterval  | String | 是    | 下单间隔，单位为秒。<br>以买入为例，市价低于 “限制价” 时，策略开始按 “时间周期” 用买一价向上取一定价距 / 比例的委托价来委托小额买单。当前这个参数就是其中的 “时间周期”。                                               |

<b>冰山委托</b>

| 参数名    | 类型   | 是否必须 | 描述                                                                                                                                          |
| :-------- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------|
| szLimit | String | 是 | 单笔最小数量限制，仅适用于 `smart_iceberg` |
| lmtOrderNumber | String | 是 | 限价拆单数量，仅适用于 `smart_iceberg` |
| aggressiveness | String | 是 | 激进度，仅适用于 `smart_iceberg`<br>`radical`：更快成交<br>`mid`：较快成交，较优价格<br>`conservative`：盘口排队 |
| pxLimit | String | 否 | 价格上限，仅适用于 `smart_iceberg` |
| triggerParams | Array of objects | 否 | 触发参数，列表为空时默认立即触发，仅适用于 `smart_iceberg` |
| > triggerAction | String | 是 | 触发行为<br>`start`：启动冰山委托 |
| > triggerStrategy | String | 是 | 触发策略<br>`instant`：立即触发<br>`price`：价格触发<br>`rsi`：RSI指标触发<br>默认为 `instant` |
| > triggerPx | String | 否 | 触发价格<br>仅在 `triggerStrategy` 为 `price` 时有效 |
| > triggerCond | String | 否 | 触发条件<br>`cross_up`：上穿<br>`cross_down`：下穿<br>`above`：上方<br>`below`：下方<br>`cross`：交叉<br>仅在 `triggerStrategy` 为 `rsi` 时有效 |
| > timeframe | String | 否 | K线种类<br>`3m`、`5m`、`15m`、`30m`（m代表分钟）<br>`1H`、`4H`（H代表小时）<br>`1D`（D代表天）<br>仅在 `triggerStrategy` 为 `rsi` 时有效 |
| > thold | String | 否 | 阈值，取值 [1,100] 的整数<br>仅在 `triggerStrategy` 为 `rsi` 时有效 |
| > timePeriod | String | 否 | RSI 计算周期，默认值为 `14`<br>仅在 `triggerStrategy` 为 `rsi` 时有效 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "algoId":"12345689",
            "clOrdId": "",
            "algoClOrdId": "",
            "sCode":"0",
            "sMsg":"",
            "tag":""
        }
    ]
}
```

#### 返回参数

| **参数名**     | **类型** | **描述**                    |
|:------------| :------- |:--------------------------|
| algoId      | String   | 策略委托单ID                   |
| clOrdId     | String   | <del>客户自定义订单ID</del>（已废弃） |
| algoClOrdId | String   | 客户自定义策略订单ID               |
| sCode       | String   | 事件执行结果的code，0代表成功         |
| sMsg        | String   | 事件执行失败时的msg               |
| tag         | String   | 订单标签                      |
