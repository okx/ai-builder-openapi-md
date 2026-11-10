---
site: okex
lang: zh
name: 获取交易产品基础信息
method: GET
path: /api/v5/public/instruments
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取交易产品基础信息 [public-data-rest-api-get-instruments]

获取所有可交易产品的信息列表。

#### 限速：20次/2s
#### 限速规则：IP + Instrument Type

#### HTTP请求

`GET /api/v5/public/instruments`

> 请求示例

```shell
GET /api/v5/public/instruments?instType=SPOT
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取交易产品基础信息
result = publicDataAPI.get_instruments(
    instType="SPOT"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须    | 描述                                                                                 |
|:---------------| :----- |:--------|:-----------------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| seriesId       | String | 可选     | 系列 ID，如 `BTC-ABOVE-DAILY`。当 `instType` 为 `EVENTS` 时必填 |
| instFamily     | String | 否       | 交易品种，仅适用于`交割`/`永续`/`期权`                                                                |
| instId         | String | 否       | 产品ID                                                                               |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
      {
            "alias": "",
            "auctionEndTime": "",
            "baseCcy": "BTC",
            "category": "1",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "expTime": "",
            "futureSettlement": false,
            "groupId": "1",
            "instFamily": "",
            "instId": "BTC-USDT",
            "instType": "SPOT",
            "lever": "10",
            "listTime": "1606468572000",
            "lotSz": "0.00000001",
            "maxIcebergSz": "9999999999.0000000000000000",
            "maxLmtAmt": "1000000",
            "maxLmtSz": "9999999999",
            "maxMktAmt": "1000000",
            "maxMktSz": "",
            "maxStopSz": "",
            "maxTriggerSz": "9999999999.0000000000000000",
            "maxTwapSz": "9999999999.0000000000000000",
            "minSz": "0.00001",
            "optType": "",
            "openType": "call_auction",
            "preMktSwTime": "",
            "quoteCcy": "USDT",
            "tradeQuoteCcyList": [
                "USDT"
            ],
            "settleCcy": "",
            "state": "live",
            "ruleType": "normal",
            "stk": "",
            "tickSz": "0.1",
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
    ]
}
```

#### 返回参数

| 参数名          | 类型   | 描述                                                                                                                                                                             |
|:-------------| :----- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | 产品类型                                                                                                                                                                           |
| seriesId     | String | 系列 ID，如 `BTC-ABOVE-DAILY`。仅适用于 `EVENTS`                                                                                                                                      |
| instId       | String | 产品id， 如 `BTC-USDT`                                                                                                                                                             |
| uly          | String | 标的指数，如 `BTC-USD`，仅适用于`杠杆/交割/永续/期权`                                                                                                                                             |
| groupId | String | 交易产品手续费分组ID<br>现货：<br>`3`：TRY现货<br>`5`：BRL现货<br>`7`：AED现货<br>`8`：AUD现货<br>`10`：SGD现货<br>`11`：零手续费现货<br>`12`：现货分组一<br>`13`：现货分组二<br>`14`：现货分组三<br>`15`: 现货特别分组<br>`17`：现货稳定币分组<br>`22`：现货RWA分组二<br><br>交割合约：<br>`5`：交割合约分组一<br>`6`：交割合约分组二<br>`8`：XPERP分组二<br>`10`：XPERP RWA分组二<br><br>永续合约：<br>`4`：永续合约分组一<br>`5`：永续合约分组二<br>`6`：SWAP RWA分组一<br>`7`：SWAP RWA分组二<br><br>期权：<br>`1`：币本位期权<br><br>**用户需要同时使用instType和groupId来确定一个交易产品的交易手续费分组；用户应该将此接口和[获取当前账户交易手续费费率](/docs-v5/zh/#trading-account-rest-api-get-fee-rates)一起使用，以获取特定交易产品的手续费率** <br><br>**部分枚举值可能不适用于您，以实际返回为准**|
| instFamily   | String | 交易品种，如 `BTC-USD`，仅适用于`杠杆/交割/永续/期权`                                                                                                                                             |
| category     | String | <del>币种类别</del>（已废弃）                                                                                                                                                           |
| baseCcy      | String | 交易货币币种，如 `BTC-USDT` 中的 `BTC` ，仅适用于`币币/币币杠杆`                                                                                                                                    |
| quoteCcy     | String | 计价货币币种，如 `BTC-USDT` 中的`USDT` ，仅适用于`币币/币币杠杆`                                                                                                                                    |
| settleCcy    | String | 盈亏结算和保证金币种，如 `BTC` 仅适用于`交割`/`永续`/`期权`                                                                                                                                              |
| ctVal        | String | 每张合约的面值。计价货币取决于 `ctType`：线性合约以标的货币计（如BTC-USDT-SWAP，ctVal=0.01 BTC）；反向合约以USD计（如BTC-USD-SWAP，ctVal=100 USD）。名义价值：线性 = 张数 × ctVal × 标记价格（计价货币）；反向 = 张数 × ctVal（USD固定）。<br>仅适用于 `FUTURES`/`SWAP`/`OPTION` |
| ctMult       | String | 合约乘数，仅适用于`交割`/`永续`/`期权`                                                                                                                                                            |
| ctValCcy     | String | 合约面值计价币种，仅适用于`交割`/`永续`/`期权`                                                                                                                                                        |
| optType      | String | 期权类型，`C`或`P` 仅适用于`期权`                                                                                                                                                          |
| stk          | String | 行权价格，仅适用于`期权`                                                                                                                                                                  |
| listTime     | String | 上线时间 <br>Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                       |
| auctionEndTime   | String | ~~集合竞价结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` <br>仅适用于通过集合竞价方式上线的`币币`，其余情况返回""（已废弃，请使用contTdSwTime）~~                                 |
| contTdSwTime | String | 连续交易开始时间，从集合竞价、提前挂单切换到连续交易的时间，Unix时间戳格式，单位为毫秒。e.g. `1597026383085`。<br>仅适用于通过集合竞价或提前挂单上线的`SPOT`/`MARGIN`，在其他情况下返回""。 |
| preMktSwTime | String	 | 盘前交易产品切换为正常交易的时间，Unix时间戳的毫秒数格式，如 `1597026383085`<br>仅适用于盘前`SWAP` 与盘前 X-Perp `FUTURES`。当盘前 X-Perp 转换为正常 X-Perp 时填充 |
| openType | String | 开盘类型<br>`fix_price`: 定价开盘<br>`pre_quote`: 提前挂单<br>`call_auction`: 集合竞价 <br>只适用于`SPOT`/`MARGIN`，其他业务线返回"" |
| expTime      | String | 产品下线时间<br>适用于`币币/杠杆/交割/永续/期权`，对于 `交割/期权`，为交割/行权日期；亦可以为产品下线时间，有变动就会推送。                                                                                                          |
| lever        | String | 交易所对该合约设定的最大杠杆上限。账户实际可用杠杆可能因VIP等级和仓位大小而更低。用户当前配置的杠杆请使用 GET /api/v5/account/leverage-info 查询。<br>不适用于 `SPOT`、`OPTION` |
| tickSz       | String | 最小价格变动单位，如 `0.0001`。<br>对于 `OPTION`/`EVENTS`，该值为 tick band 中的最小 tickSz。如需获取各价格区间的精确 tickSz，请使用"获取期权价格梯度"接口并传入对应的 `instType` 参数。 |
| lotSz        | String | 合约面值最小变动单位（委托量步长），所有委托量（sz）必须为 `lotSz` 的整数倍，违反则返回错误51121。下单数量精度<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                           |
| minSz        | String | 最小委托量。委托量必须同时满足：sz ≥ `minSz` 且 sz 为 `lotSz` 的整数倍。最小下单数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                           |
| ctType       | String | 合约类型<br>`linear`：正向合约，保证金、盈亏及结算均以计价货币计（如BTC-USDT-SWAP以USDT计）。<br>`inverse`：反向合约，保证金、盈亏及结算均以标的货币计（如BTC-USD-SWAP以BTC计）。反向合约的USD盈亏为非线性：固定BTC盈亏的USD价值随BTC价格变化。<br>仅适用于 `FUTURES`/`SWAP` |
| alias        | String | 合约日期别名（已废弃，将于 2026 年 4 月底下线，请使用 expTime 字段获取交割时间）<br>`this_week`：本周<br>`next_week`：次周<br>`this_month`：本月<br>`next_month`：次月<br>`quarter`：季度<br>`next_quarter`：次季度<br>`third_quarter`：第三季度<br>`this_five_years`：当期五年合约<br>`next_five_years`：次期五年合约<br>仅适用于`交割` |
| state        | String | 产品状态<br>`live`：交易中 <br>`suspend`：暂停中<br>`rebase`：合约在变基中，不可交易<br>`post_only`：仅接受 post-only 订单；已有 post-only 订单可改单和撤单。其他订单类型（市价单、IOC、FOK、普通限价单）将被拒绝。<br>`preopen`：预上线，交割和期权合约轮转生成到开始交易；部分交易产品上线前<br>`test`：测试中（测试产品，不可交易）<br>`settling`：结算中，仅适用于 `EVENTS`                                                            |
| ruleType | String | 交易规则类型<br>`normal`：普通交易<br>`pre_market`：盘前交易，含盘前 X-Perp `FUTURES`<br>`rebase_contract`：盘前变基合约<br>`xperp`：永续合约风格的交割合约，仅适用于部分 `FUTURES` 合约。盘前 X-Perp 转换为正常 X-Perp 后，由 `pre_market` 变为 `xperp` |
| maxLmtSz     | String | 限价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                     |
| maxMktSz     | String | 市价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                                                                                                     |
| maxLmtAmt    | String | 限价单的单笔最大美元价值                                                                                                                                                                   |
| maxMktAmt    | String | 市价单的单笔最大美元价值<br>仅适用于`币币/币币杠杆`                                                                                                                                                  |
| maxTwapSz    | String | 时间加权单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`。<br/>单笔最小委托数量为 minSz*2                                                                                                                 |
| maxIcebergSz | String | 冰山委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                    |
| maxTriggerSz | String | 计划委托委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                  |
| maxStopSz    | String | 止盈止损市价委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                                                                                                |
| futureSettlement | Boolean | 交割合约是否支持每日结算<br>适用于`全仓``交割`                                                                                 |
| tradeQuoteCcyList | Array of strings	 | 可用于交易的计价币种列表，如 ["USD", "USDC”]. |
| instIdCode | Integer | 产品唯一标识代码。<br>对于简单二进制编码，您必须使用 `instIdCode` 而不是 `instId`。<br>对于同一`instId`，实盘和模拟盘的值可能会不一样。 <br> 当值还未生成时，返回 `null`。 |
| instCategory | String | 标的资产类别（产品ID的第一部分）。例如：对于 `BTC-USDT-SWAP`，instCategory 表示 `BTC` 所属的资产类别。<br>`1`: 加密货币 <br>`3`: 股票类资产 <br>`4`: 大宗商品 <br>`5`: 外汇 <br>`6`: 债券 <br>`""` 当值不可用时返回空字符串 |
| initPxLmtPct | String | 合约上线后前 10 分钟内的初始价格限制区间，小数百分比，例如 `0.05` 代表 5%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| floatPxLmtPct | String | 常规交易期间的浮动价格限制区间，小数百分比，例如 `0.03` 代表 3%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| maxPxLmtPct | String | 最大价格限制上限（下单价格相对指数价格偏离的硬性上限），小数百分比，例如 `0.15` 代表 15%。通过 GET /api/v5/public/price-limit 可获取对应价格限制。<br>适用于 `SPOT`/`MARGIN`/`SWAP`/`FUTURES`；`OPTION` 和 `EVENTS` 返回 `""`。 |
| rpiMinLevel | String | RPI 买卖间最小间距，以有机订单的价格档位计。 |
| rpiMinPxBand | String | 距对手方有机最优价的最小距离，以基点（bps）计。 |
| upcChg | Array of objects | 即将变更的参数列表。当没有即将变更的参数时，返回空数组 []|
| > param | String | 即将变更的参数名称。<br> `tickSz`<br> `minSz`：若为交割/永续合约（`FUTURES`/`SWAP`），`lotSz` 会同步变更。<br> `maxMktSz`|
| > newValue | String | 即将变更的参数值。|
| > effTime | String | 生效时间。Unix 时间戳格式，例如 `1597026383085`|

<aside class="notice">
当合约预上线时，状态变更为预上线（即新生成一个合约，新合约会处于预上线状态）；
</aside>

<aside class="notice">
listTime以及contTdSwTime<br>
对于通过集合竞价/提前挂单方式上线的币币，listTime为集合竞价/提前挂单的开始时间，contTdSwTime为集合竞价/提前挂单的结束时间、连续交易的开始时间；对于其他情况及业务线，listTime即为连续交易开始时间，contTdSwTime将返回""
</aside>

<aside class="notice">
state<br>
对于`币币`、`杠杆`、`永续`和`交割`，状态state在时间到达listTime时由`preopen`转变为`live`。对于`期权`合约，由于内部处理原因，状态可能在`listTime`之后短暂延迟变为`live`。建议在下单前确认`state`为`live`。<br>
当产品下线的时候（如交割合约被交割的时候，期权合约被行权的时候），查询不到该产品
</aside>

<aside class="notice">
产品下线公告一经发出，接口及频道会更新下线时间(expTime)。<br>
产品上线公告一经发出，接口及频道会更新上线时间：<br>

1. 对于币币/杠杆/永续， 该事件仅适用于产品类型(instType), 交易产品ID(instId), 上线时间(listTime), 产品状态(state)字段；<br>
2. 对于交割，该事件仅适用于产品类型(instType), 交易品种(instFamily), 上线时间(listTime), 产品状态(state)字段；<br>
3. 其他字段暂时为空，会比上线时间至少提前 5 分钟更新完整，然后 WebSocket 才会支持通过对应的交易产品ID/交易品种进行订阅。<br>
</aside>
