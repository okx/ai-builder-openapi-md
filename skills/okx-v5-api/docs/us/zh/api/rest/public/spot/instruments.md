---
site: us
lang: zh
name: 获取交易产品基础信息
method: GET
path: /api/v5/public/instruments
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取交易产品基础信息 [public-data-rest-api-get-instruments]

获取欧意平台所有可交易产品的信息列表。查看当前用户可以交易的产品信息列表参考[获取交易产品基础信息](/docs-v5/zh/#trading-account-rest-api-get-instruments)。

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

| 参数名            | 类型   | 是否必须    | 描述                                                                                |
|:---------------| :----- |:--------|:----------------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`SPOT`：币币 |
| instId         | String | 否       | 产品ID                                                                              |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
      {
            "alias": "",
            "baseCcy": "BTC",
            "category": "1",
            "ctMult": "",
            "ctType": "",
            "ctVal": "",
            "ctValCcy": "",
            "contTdSwTime": "1704876947000",
            "expTime": "",
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
            "settleCcy": "",
            "state": "live",
            "stk": "",
            "tickSz": "0.1",
            "uly": ""
        }
    ]
}
```

#### 返回参数

| 参数名          | 类型   | 描述                                                                                                                                                                             |
|:-------------| :----- |:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | 产品类型                                                                                                                                                                           |
| instId       | String | 产品id， 如 `BTC-USDT`                                                                                                                                                             |
| uly          | String | 标的指数，如 `BTC-USD`，仅适用于`交割`/`永续`/`期权`                                                                                                                                                |
| groupId | String | 交易产品手续费分组ID<br>现货：<br>`3`：TRY现货<br>`5`：BRL现货<br>`7`：AED现货<br>`8`：AUD现货<br>`10`：SGD现货<br>`11`：零手续费现货<br>`12`：现货分组一<br>`13`：现货分组二<br>`14`：现货分组三<br>`15`: 现货特别分组<br>`17`：现货稳定币分组<br>`22`：现货RWA分组二<br><br>交割合约：<br>`5`：交割合约分组一<br>`6`：交割合约分组二<br>`8`：XPERP分组二<br>`10`：XPERP RWA分组二<br><br>永续合约：<br>`4`：永续合约分组一<br>`5`：永续合约分组二<br>`6`：SWAP RWA分组一<br>`7`：SWAP RWA分组二<br><br>期权：<br>`1`：币本位期权<br><br>**用户需要同时使用instType和groupId来确定一个交易产品的交易手续费分组；用户应该将此接口和[获取当前账户交易手续费费率](/docs-v5/zh/#trading-account-rest-api-get-fee-rates)一起使用，以获取特定交易产品的手续费率** <br><br>**部分枚举值可能不适用于您，以实际返回为准**|
| instFamily   | String | 交易品种，如 `BTC-USD`，仅适用于`交割`/`永续`/`期权`                                                                                                                                                |
| category     | String | <del>币种类别</del>（已废弃）                                                                                                                                                           |
| baseCcy      | String | 交易货币币种，如 `BTC-USDT` 中的 `BTC` ，仅适用于`币币/币币杠杆`                                                                                                                                    |
| quoteCcy     | String | 计价货币币种，如 `BTC-USDT` 中的`USDT` ，仅适用于`币币/币币杠杆`                                                                                                                                    |
| settleCcy    | String | 盈亏结算和保证金币种，如 `BTC` 仅适用于`交割`/`永续`/`期权`                                                                                                                                              |
| ctVal        | String | 合约面值，仅适用于`交割`/`永续`/`期权`                                                                                                                                                            |
| ctMult       | String | 合约乘数，仅适用于`交割`/`永续`/`期权`                                                                                                                                                            |
| ctValCcy     | String | 合约面值计价币种，仅适用于`交割`/`永续`/`期权`                                                                                                                                                        |
| optType      | String | 期权类型，`C`或`P` 仅适用于`期权`                                                                                                                                                          |
| stk          | String | 行权价格，仅适用于`期权`                                                                                                                                                                  |
| listTime     | String | 上线时间 <br>Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                       |
| auctionEndTime   | String | ~~集合竞价结束时间，Unix时间戳的毫秒数格式，如 `1597026383085` <br>仅适用于通过集合竞价方式上线的`币币`，其余情况返回""（已废弃，请使用contTdSwTime）~~                                 |
| contTdSwTime | String | 连续交易开始时间，从集合竞价、提前挂单切换到连续交易的时间，Unix时间戳格式，单位为毫秒。e.g. `1597026383085`。<br>仅适用于通过集合竞价或提前挂单上线的`SPOT`/`MARGIN`，在其他情况下返回""。 |
| preMktSwTime | String	 | 盘前永续合约转为普通永续合约的时间，Unix时间戳的毫秒数格式，如 `1597026383085`<br>仅适用于盘前`SWAP` |
| openType | String | 开盘类型<br>`fix_price`: 定价开盘<br>`pre_quote`: 提前挂单<br>`call_auction`: 集合竞价 <br>只适用于`SPOT`/`MARGIN`，其他业务线返回"" |
| expTime      | String | 产品下线时间<br>适用于`币币/杠杆/交割/永续/期权`，对于 `交割/期权`，为交割/行权日期；亦可以为产品下线时间，有变动就会推送。                                                                                                          |
| lever        | String | 该`instId`支持的最大杠杆倍数，不适用于`币币`、`期权`                                                                                                                                               |
| tickSz       | String | 下单价格精度，如 `0.0001`<br>对于期权来说，是梯度中的最小下单价格精度，如果想要获取期权价格梯度，请使用"获取期权价格梯度"接口                                                                                                         |
| lotSz        | String | 下单数量精度<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                           |
| minSz        | String | 最小下单数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                           |
| ctType       | String | 合约类型<br>`linear`：正向合约<br>`inverse`：反向合约<br>仅适用于`交割/永续`                                                                                                                         |
| alias        | String | 合约日期别名<br>`this_week`：本周 <br>`next_week`：次周 <br>`this_month`：本月 <br>`next_month`：次月<br>`quarter`：季度<br>`next_quarter`：次季度<br>`third_quarter`：第三季度 <br>仅适用于`交割` <br>**不建议使用，用户应通过 expTime 字段获取合约的交割日期** |
| state        | String | 产品状态<br>`live`：交易中 <br>`suspend`：暂停中<br>`preopen`：预上线，交割和期权合约轮转生成到开始交易；部分交易产品上线前<br>`test`：测试中（测试产品，不可交易）                                                            |
| maxLmtSz     | String | 限价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                     |
| maxMktSz     | String | 市价单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                                                                                                     |
| maxLmtAmt    | String | 限价单的单笔最大美元价值                                                                                                                                                                   |
| maxMktAmt    | String | 市价单的单笔最大美元价值<br>仅适用于`币币/币币杠杆`                                                                                                                                                  |
| maxTwapSz    | String | 时间加权单的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                   |
| maxIcebergSz | String | 冰山委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                    |
| maxTriggerSz | String | 计划委托委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`交易货币`                                                                                                                                  |
| maxStopSz    | String | 止盈止损市价委托的单笔最大委托数量<br>合约的数量单位是`张`，现货的数量单位是`USDT`                                                                                                                                |

<aside class="notice">
当合约预上线时，状态变更为预上线（即新生成一个合约，新合约会处于预上线状态）；
当产品下线的时候（如交割合约被交割的时候，期权合约被行权的时候），查询不到该产品
</aside>

<aside class="notice">
listTime以及contTdSwTime<br>
对于通过集合竞价/提前挂单方式上线的币币，listTime为集合竞价/提前挂单的开始时间，contTdSwTime为集合竞价/提前挂单的结束时间、连续交易的开始时间；对于其他情况及业务线，listTime即为连续交易开始时间，contTdSwTime将返回""
</aside>
