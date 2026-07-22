---
site: turkey
lang: zh
name: POST / 下单
method: POST
path: /api/v5/trade/order
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 下单 [order-book-trading-trade-post-place-order]

只有当您的账户有足够的资金才能下单。

#### 限速：60次/2s
#### 限速规则：User ID + Instrument ID

#### HTTP请求

`POST /api/v5/trade/order`

> 请求示例

```shell
# 币币下单
POST /api/v5/trade/order
body
{
    "instId":"BTC-USDT",
    "tdMode":"cash",
    "clOrdId":"b15",
    "side":"buy",
    "ordType":"limit",
    "px":"2.15",
    "sz":"2"
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

# 现货模式限价单
result = tradeAPI.place_order(
    instId="BTC-USDT",
    tdMode="cash",
    clOrdId="b15",
    side="buy",
    ordType="limit",
    px="2.15",
    sz="2"
)

print(result)
```

#### 请求参数

| 参数名                    | 类型              | 是否必须     | 描述                                                                                                               |
|:-----------------------|:----------------|:---------|:-----------------------------------------------------------------------------------------------------------------|
| instId                 | String          | 是        | 产品ID，如 `BTC-USDT`                                                                                                |
| tdMode                 | String          | 是        | 交易模式<br>`cash`：非保证金                                                                                              |
| clOrdId                | String          | 否        | 客户自定义订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                             |
| tag                    | String          | 否        | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                                                  |
| side                   | String          | 是        | 订单方向<br>`buy`：买<br>`sell`：卖                                                                                      |
| ordType                | String          | 是        | 订单类型<br>`market`：市价单<br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`fok`：全部成交或立即取消 <br>`ioc`：立即成交并取消剩余 <br>`elp`：流动性增强计划订单             |
| sz                     | String          | 是        | 委托数量                                                                                                             |
| px                     | String          | 可选       | 委托价格，仅适用于`limit`、`post_only`、`fok`、`ioc`类型的订单                                                                    |
| tgtCcy                 | String          | 否        | 市价单委托数量`sz`的单位，仅适用于`币币`市价订单<br>`base_ccy`: 交易货币<br>`quote_ccy`：计价货币<br>买单默认`quote_ccy`， 卖单默认`base_ccy`           |
| banAmend               | Boolean         | 否        | 是否禁止币币市价改单，true 或 false，默认false <br>为true时，余额不足时，系统不会改单，下单会失败，仅适用于币币市价单                                          |
| stpId                  | String          | 否        | ~~自成交保护ID。来自同一个母账户配着同一个ID的订单不能自成交<br>用户自定义1<=x<=999999999的整数~~（已弃用）                                                       |
| stpMode                | String          | 否        | 自成交保护模式<br>默认为 cancel maker <br>`cancel_maker`,`cancel_taker`,`cancel_both`<br>Cancel both不支持FOK |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| isElpTakerAccess | Boolean | 否 | 是否作为 taker 吃单 ELP<br>`true`：该请求能吃单 ELP，但会被施加延迟<br>`false`：该请求不能吃单 ELP，并且没有延迟<br><br>默认值为`false`，`true`仅适用于ioc订单 |
| attachAlgoOrds         | Array of objects | 否        | 附带止盈止损或移动止盈止损订单信息                                                                                                       |
| > attachAlgoClOrdId    | String          | 否        | 下单附带止盈止损或移动止盈止损时，客户自定义的策略订单ID <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>订单完全成交，下附带策略委托单时，该值会传给`algoClOrdId`        |
| > tpTriggerPx          | String          | 可选       | 止盈触发价，如果填写此参数，必须填写 止盈委托价                                                                                         |
| > tpOrdPx              | String          | 可选       | 止盈委托价，如果填写此参数，必须填写 止盈触发价<br>委托价格为-1时，执行市价止盈                                                                      |
| > slTriggerPx          | String          | 可选       | 止损触发价，如果填写此参数，必须填写 止损委托价                                                                                         |
| > slOrdPx              | String          | 可选       | 止损委托价，如果填写此参数，必须填写 止损触发价<br>委托价格为-1时，执行市价止损                                                                      |
| > tpTriggerPxType      | String          | 否        | 止盈触发价类型<br>`last`：最新价格<br>默认为`last`                                               |
| > slTriggerPxType      | String          | 否        | 止损触发价类型<br>`last`：最新价格格<br>默认为`last`                                               |
| > sz                   | String          | 可选       | 数量。仅适用于“多笔止盈”的止盈订单，且对于“多笔止盈”的止盈订单必填                                                                              |
| > amendPxOnTriggerType | String          | 否        | 是否启用开仓价止损，仅适用于分批止盈的止损订单，第一笔止盈触发时，止损触发价格是否移动到开仓均价止损<br>`0`：不开启，默认值 <br>`1`：开启，且止损触发价不能为空                          |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "clOrdId":"oktswap6",
            "ordId":"12345689",
            "tag":"",
            "sCode":"0",
            "sMsg":"",
            "subCode":""
        }
    ],
    "inTime": "1695190491421339",
    "outTime": "1695190491423240"
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                               |
| :--------- | :------- |:---------------------------------------------------------------------|
| code       | String   | 结果代码，`0`表示成功                                                         |
| msg        | String   | 错误信息，代码为0时，该字段为空                                                     |
| data       | Array of objects   | 包含结果的对象数组                                                            |
| > ordId      | String   | 订单ID                                                                 |
| > clOrdId    | String   | 客户自定义订单ID                                                            |
| > tag        | String   | 订单标签                                                                 |
| > sCode      | String   | 事件执行结果的code，0代表成功                                                    |
| > sMsg       | String   | 事件执行失败或成功时的msg                                                       |
| > subCode    | String   | sCode 的子码。<br>当 sCode 为 0（请求成功）时，返回 `""`。<br>当 sCode 不为 0（请求失败）且存在子码时，返回对应的子码；若无子码，则返回 `""`。|
| inTime | String | REST网关接收请求时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`<br>返回的时间是请求验证后的时间。|
| outTime | String | REST网关发送响应时的时间戳，Unix时间戳的微秒数格式，如 `1597026383085123`                   |

<aside class="notice">
clOrdId<br>
clOrdId 是用户在 User ID 维度自定义的订单唯一标识符。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。<br>
clOrdId不能与当前所有的挂单的clOrdId重复
</aside>

<aside class="notice">
ordType<br>
订单类型，创建新订单时必须指定，您指定的订单类型将影响需要哪些订单参数和撮合系统如何执行您的订单，以下是有效的ordType：<br>
普通委托：<br>
limit：限价单，要求指定sz 和 px<br>
market：市价单，币币和币币杠杆，是市价委托吃单；交割合约和永续合约，是自动以最高买/最低卖价格委托，遵循限价机制；期权合约不支持市价委托；由于市价委托无法确定成交价格，为确保有足够的资产买入设定数量的交易币种，会多冻结5%的计价币资产<br>
高级委托：<br>
post_only：限价委托，在下单那一刻只做maker，如果该笔订单的任何部分会吃掉当前挂单深度，则该订单将被全部撤销。<br>
fok：限价委托，全部成交或立即取消，如果无法全部成交该笔订单，则该订单将被全部撤销。<br>
ioc：限价委托，立即成交并取消剩余，立即按照委托价格撮合成交，并取消该订单剩余未完成数量，不会在深度列表上展示委托数量。<br>
optimal_limit_ioc：市价委托，立即成交并取消剩余，仅适用于交割合约和永续合约。
</aside>

<aside class="notice">
sz<br>
交易数量，表示要购买或者出售的数量。<br>
当币币以限价买入和卖出时，指交易货币数量。<br>
当币币以市价买入时，指计价货币的数量。<br>
当币币以市价卖出时，指交易货币的数量。<br>
对于币币市价单，单位由 tgtCcy 决定
</aside>

<aside class="notice">
tgtCcy<br>
市价单委托数量`sz`的单位：仅适用于币币市价下单交易。<br>
交易货币：base_ccy<br>
计价货币：quote_ccy
<br>

您在使用交易货币买入或者计价货币卖出时，请知晓：
<br>
1.如果您输入的数量大于当前可买或者可卖的数量，系统将按照您的最大可买或者可卖数量帮您完成交易，如果您希望按照指定数量成交，那您可以尝试使用限价单，等待市场价格波动到锁定的余额可以买入或卖出您指定的数量。
<br>
2.如果您输入的数量不大于当前可买或者可卖的数量，那当市场价格波动过大时，锁定的余额可能没办法买入您输入的交易货币数量或卖出您输入的计价货币数量，为保证您的交易体验，我们基于【能买多少买多少】或者【能卖多少卖多少】的原则，更改下单的数量帮您完成交易。此外，我们将尽量多锁定一点余额来规避更改下单数量的情况。
<br>
2.1 交易币买入例子：
<br>
以市价下单 买入 10个LTC为例，用户可买为11个，此时 10 < 11，挂单成功。当LTC-USDT的市价为200，用户被锁定余额为3,000 USDT，200*10 < 3,000，最终成交10个LTC；
若市场波动过大，LTC-USDT的市价为400，此时400*10 > 3,000，当用户被锁定的余额不够买入下单指定的交易货币数量时，系統使用用户被锁定的最大余额3,000 USDT下单买入，最终成交 3,000/400 = 7.5个 LTC。
<br>
2.2 计价币卖出例子：
<br>
以市价下单 卖出 1,000USDT为例，用户可卖为1,200USDT，1,000 < 1,200，挂单成功。LTC-USDT的市价为200，用户被锁定的余额为6个LTC，最终成交5个LTC；
若市场波动过大，LTC-USDT的市价为100，100*6 < 1,000，当用户被锁定的余额不够卖出下单指定的计价货币数量时，系統使用用户被锁定的最大余额6个LTC下单，最终成交 6 * 100 = 600 USDT。
</aside>

<aside class="notice">
下单附带止盈止损，只有当该订单完全成交时，才会生成止盈止损策略订单，否则不会生成止盈止损策略订单。
</aside>

<aside class="notice">
强制自成交保护<br>
交易系统会以母账户维度实施强制自成交保护，同一母账户下所有账户，包括母账户本身和所有子账户，都无法进行自成交。订单的默认STP模式为`Cancel Maker`，用户亦可以通过下单接口的stpMode参数指定订单的STP模式。<br>
强制自成交保护不会导致延迟。<br>
有三种STP模式。STP模式始终基于taker订单中的配置。<br>
1.Cancel Maker：这是默认的STP模式，系统撤Maker订单以防止自成交。然后，taker订单会基于深度继续和下一个订单成交。<br>
2.Cancel Taker：撤Taker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后撤单。FOK订单会确保完全成交和自成交保护。<br>
3.Cancel Both：撤Taker和Maker订单以防止自成交。如果用户的Maker订单不是深度里第一个订单，Taker订单会被部分成交，然后Taker订单的剩余数量和第一个自我Maker订单被取消。此模式不支持FOK订单。<br>
</aside>

<aside class="notice">
isElpTakerAccess:true订单限速<br>
- 50个/2s，限制维度为 User ID + Instrument ID<br>
- 该限速会在 REST 和 WebSocket 的下单及批量下单接口中共享
</aside>
