---
site: turkey
lang: zh
name: 获取交易产品基础信息
method: GET
path: /api/v5/account/instruments
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取交易产品基础信息 [trading-account-rest-api-get-instruments]

获取当前账户可交易产品的信息列表。

#### 限速：20次/2s
#### 限速规则：User ID + Instrument Type

#### HTTP请求

`GET /api/v5/account/instruments`

> 请求示例

```shell
GET /api/v5/account/instruments?instType=SPOT
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.get_instruments(instType="SPOT")
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须    | 描述                                                                                 |
|:---------------| :----- |:--------|:-----------------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| seriesId       | String | 可选     | 系列 ID，如 `BTC-ABOVE-DAILY`。当 instType 为 `EVENTS` 时必填 |
| instFamily     | String | 可选       | 交易品种，仅适用于`交割`/`永续`/`期权`，期权必填                                                                |
| instId         | String | 否       | 产品ID                                                                               |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "elp": "0",
            "expTime": "",
            "futureSettlement": false,
            "groupId": "4",
            "instFamily": "",
            "instId": "BTC-EUR",
            "instType": "SPOT",
            "lever": "",
            "listTime": "1704876947000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "1000000",
            "maxPlatOILmt": "1000000000",
            "maxPlatOICoinLmt": "",
            "maxStopSz": "1000000",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "preMktSwTime": "",
            "posLmtPct": "30",
            "posLmtAmt": "2500000",
            "quoteCcy": "EUR",
            "tradeQuoteCcyList": [
                "EUR"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "1",
            "uly": "",
            "instIdCode": 1000000000,
            "instCategory": "1",
            "initPxLmtPct": "0.05",
            "floatPxLmtPct": "0.03",
            "maxPxLmtPct": "0.15",
            "upcChg": [
                {
                    "param": "tickSz",
                    "newValue": "0.0001",
                    "effTime": "1704876947000"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名              | 类型   | 描述                                                                                                        |
|:-----------------| :----- |:----------------------------------------------------------------------------------------------------------|
| instType         | String | 产品类型                                                                                                      |
| seriesId         | String | 系列 ID，如 `BTC-ABOVE-DAILY`。仅适用于 `EVENTS`                                                                    |
| instId           | String | 产品id， 如 `BTC-USDT`                                                                                        |
| uly              | String | 标的指数，如 `BTC-USD`，仅适用于`杠杆/交割/永续/期权`                                                                        |
| groupId | String | 交易产品手续费分组ID<br>现货：<br>`3`：TRY现货<br>`5`：BRL现货<br>`7`：AED现货<br>`8`：AUD现货<br>`10`：SGD现货<br>`11`：零手续费现货<br>`12`：现货分组一<br>`13`：现货分组二<br>`14`：现货分组三<br>`15`: 现货特别分组<br>`17`：现货稳定币分组<br><br>交割合约：<br>`5`：交割合约分组一<br>`6`：交割合约分组二<br>`8`：XPERP分组二<br>`10`：XPERP RWA分组二<br><br>永续合约：<br>`4`：永续合约分组一<br>`5`：永续合约分组二<br>`6`：SWAP RWA分组一<br>`7`：SWAP RWA分组二<br><br>期权：<br>`1`：币本位期权<br><br>**用户需要同时使用instType和groupId来确定一个交易产品的交易手续费分组；用户应该将此接口和[获取当前账户交易手续费费率](/docs-v5/zh/#trading-account-rest-api-get-fee-rates)一起使用，以获取特定交易产品的手续费率** <br><br>**部分枚举值可能不适用于您，以实际返回为准**|
| instFamily       | String | 交易品种，如 `BTC-USD`，仅适用于`杠杆/交割/永续/期权`                                                                        |
| baseCcy          | String | 交易货币币种，如 `BTC-USDT` 中的 `BTC` ，仅适用于`币币/币币杠杆`                                                               |
| quoteCcy         | String | 计价货币币种，如 `BTC-USDT` 中的`USDT` ，仅适用于`币币/币币杠杆`                                                               |
| settleCcy        | String | 盈亏结算和保证金币种，如 `BTC` 仅适用于`交割`/`永续`/`期权`                                                                         |
| ctVal            | String | 合约面值，仅适用于`交割`/`永续`/`期权`                                                                                       |
| ctMult           | String | 合约乘数，仅适用于`交割`/`永续`/`期权`                                                                                       |
| ctValCcy         | String | 合约面值计价币种，仅适用于`交割`/`永续`/`期权`                                                                                   |
| optType          | String | 期权类型，`C`或`P` 仅适用于`期权`                                                                                     |
| stk              | String | 行权价格，仅适用于`期权`                                                                                             |
| listTime         | String | 上线时间 <br>Unix时间戳的毫秒数格式，如 `1597026383085`                                                                  |
| auctionEndTime   | String | ~~集合竞价结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` <br>仅适用于通过集合竞价方式上线的`币币`，其余情况返回""（已废弃，请使用contTdSwTime）~~                                 |
| contTdSwTime | String | 连续交易开始时间，从集合竞价、提前挂单切换到连续交易的时间，Unix时间戳格式，单位为毫秒。e.g. `1597026383085`。<br>仅适用于通过集合竞价或提前挂单上线的`SPOT`/`MARGIN`，在其他情况下返回""。 |
| preMktSwTime | String	 | 盘前交易产品切换为正常交易的时间，Unix时间戳的毫秒数格式，如 `1597026383085`<br>仅适用于盘前`SWAP` 与盘前 X-Perp `FUTURES`。当盘前 X-Perp 转换为正常 X-Perp 时填充 |
| openType | String | 开盘类型<br>`fix_price`: 定价开盘<br>`pre_quote`: 提前挂单<br>`call_auction`: 集合竞价 <br>只适用于`SPOT`/`MARGIN`，其他业务线返回"" |
| elp | String | ELP 下单权限<br>`0`：该币对不支持 ELP<br>`1`：该币对支持 ELP 但用户没有权限为其下 ELP 订单<br>`2`：该币对支持 ELP 且用户有权限为其下 ELP 订单<br><br>`1/2`不代表深度中一定有 ELP 挂单 |
| expTime          | String | 产品下线时间<br>适用于`币币/杠杆/交割/永续/期权`，对于 `交割/期权`，为交割/行权日期；亦可以为产品下线时间，有变动就会推送。                                     |
| lever            | String | 该`instId`支持的最大杠杆倍数，不适用于`币币`、`期权`                                                                          |
| tickSz           | String | 下单价格精度，如 `0.0001`。<br>对于 `OPTION`/`EVENTS`，该值为 tick band 中的最小 tickSz。如需获取各价格区间的精确 tickSz，请使用"获取期权价格梯度"接口并传入对应的 `instType` 参数。                                    |
| lotSz            | String | 下单数量精度<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                      |
| minSz            | String | 最小下单数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                      |
| ctType           | String | 合约类型<br>`linear`：正向合约<br>`inverse`：反向合约<br>仅适用于`交割/永续`                                                    |
| state            | String | 产品状态<br>`live`：交易中 <br>`suspend`：暂停中<br>`rebase`：合约在变基中，不可交易，仅适用于`SWAP`<br>`post_only`：仅接受 post-only 订单；已有 post-only 订单可改单和撤单。其他订单类型（市价单、IOC、FOK、普通限价单）将被拒绝。仅适用于 `SWAP`<br>`preopen`：预上线，交割和期权合约轮转生成到开始交易；部分交易产品上线前<br>`test`：测试中（测试产品，不可交易）<br>`settling`：结算中，仅适用于 `EVENTS` |
| ruleType         | String | 交易规则类型<br>`normal`：普通交易<br>`pre_market`：盘前交易，含盘前 X-Perp `FUTURES`<br>`rebase_contract`：盘前变基合约<br>`xperp`：永续合约风格的交割合约，仅适用于部分 `FUTURES` 合约。盘前 X-Perp 转换为正常 X-Perp 后，由 `pre_market` 变为 `xperp` |
| posLmtAmt        | String | 单一用户（母子账户共享）层面的该产品最大持仓名义价值（USD），按同方向已持仓与挂单的美元名义价值计算。单用户有效上限为 max(posLmtAmt, oiUSD × posLmtPct)。适用于 `SWAP`/`FUTURES`。 |
| posLmtPct        | String | 单一用户（母子账户共享）相对于平台当前总持仓名义价值可持有的最大比例（如 30 表示 30%）。单用户有效上限为 max(posLmtAmt, oiUSD × posLmtPct)。适用于 `SWAP`/`FUTURES`。           |
| maxPlatOILmt     | String | 该产品的全平台最大持仓名义价值（USD）。当平台总持仓量（USD）达到或超过该值时，系统将拒绝所有用户对该产品的新开仓委托；否则订单通过校验。<br>适用于 `SWAP`/`FUTURES`                     |
| maxPlatOICoinLmt | String | 该产品的全平台最大持仓名义价值（币量）。当平台总持仓量（币量）达到或超过该值时，系统将拒绝所有用户对该产品的新开仓委托；否则订单通过校验。<br>适用于 `SWAP`/`FUTURES`                     |
| longPosRemainingQuota | String | 单一用户维度（母子账户共享），在该产品下扣除已有多头仓位及挂单中的买入订单后，仍可开立的多头仓位剩余额度（以 USD 计）。|
| shortPosRemainingQuota| String | 单一用户维度（母子账户共享），在该产品下扣除已有空头仓位及挂单中的卖出订单后，仍可开立的空头仓位剩余额度（以 USD 计）。 |
| maxLmtSz         | String | 限价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                |
| maxMktSz         | String | 市价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                                |
| maxLmtAmt        | String | 限价单的单笔最大美元价值                                                                                              |
| maxMktAmt        | String | 市价单的单笔最大美元价值<br>仅适用于`币币/币币杠杆`                                                                             |
| maxTwapSz        | String | 时间加权单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币` <br/>单笔最小委托数量为 minSz*2                                       |
| maxIcebergSz     | String | 冰山委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                               |
| maxTriggerSz     | String | 计划委托委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                             |
| maxStopSz        | String | 止盈止损市价委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                           |
| futureSettlement | Boolean | 交割合约是否支持每日结算<br>适用于`全仓``交割`                                                                                 |
| tradeQuoteCcyList | Array of strings	 | 可用于交易的计价币种列表，如 ["USD", "USDC"]. |
| instIdCode | Integer | 产品唯一标识代码。<br>对于简单二进制编码，您必须使用 `instIdCode` 而不是 `instId`。<br>对于同一`instId`，实盘和模拟盘的值可能会不一样。 <br> 当值还未生成时，返回 `null`。 |
| instCategory | String | 标的资产类别（产品ID的第一部分）。例如：对于 `BTC-USDT-SWAP`，instCategory 表示 `BTC` 所属的资产类别。<br>`1`: 加密货币 <br>`3`: 股票类资产 <br>`4`: 大宗商品 <br>`5`: 外汇 <br>`6`: 债券 <br>`""` 当值不可用时返回空字符串 |
| initPxLmtPct | String | 合约上线后前 10 分钟内的初始价格限制区间，小数百分比，例如 `0.05` 代表 5%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| floatPxLmtPct | String | 常规交易期间的浮动价格限制区间，小数百分比，例如 `0.03` 代表 3%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| maxPxLmtPct | String | 最大价格限制上限（下单价格相对指数价格偏离的硬性上限），小数百分比，例如 `0.15` 代表 15%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| upcChg | Array of objects | 即将变更的参数列表。当没有即将变更的参数时，返回空数组 []|
| > param | String | 即将变更的参数名称。<br> `tickSz`<br> `minSz`：若为交割/永续合约（`FUTURES`/`SWAP`），`lotSz` 会同步变更。<br> `maxMktSz`|
| > newValue | String | 即将变更的参数值。|
| > effTime | String | 生效时间。Unix 时间戳格式，例如 `1597026383085`|

<aside class="notice">
listTime以及contTdSwTime<br>
对于通过集合竞价/提前挂单方式上线的币币，listTime为集合竞价/提前挂单的开始时间，contTdSwTime为集合竞价/提前挂单的结束时间、连续交易的开始时间；对于其他情况及业务线，listTime即为连续交易开始时间，contTdSwTime将返回""
</aside>

<aside class="notice">
state<br>
对于`币币`、`杠杆`、`永续`和`交割`，状态state在时间到达listTime时由`preopen`转变为`live`。对于`期权`合约，由于内部处理原因，状态可能在`listTime`之后短暂延迟变为`live`。建议在下单前确认`state`为`live`。<br>
当产品下线的时候（如交割合约被交割的时候，期权合约被行权的时候），查询不到该产品
</aside>
