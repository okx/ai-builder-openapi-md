---
site: okex
lang: zh
name: 查看持仓信息
method: GET
path: /api/v5/account/positions
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看持仓信息 [trading-account-rest-api-get-positions]

获取该账户下拥有实际持仓的信息。账户为买卖模式会显示净持仓（`net`），账户为开平仓模式下会分别返回开多（`long`）或开空（`short`）的仓位。按照仓位创建时间倒序排列。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/positions`

> 请求示例

```shell
# 查看BTC-USDT的持仓信息
GET /api/v5/account/positions?instId=BTC-USDT

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看持仓信息
result = accountAPI.get_positions()
print(result)
```

#### 请求参数 

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                                   |
| :------- | :----- | :------- |:-------------------------------------------------------------------------------------------------------------------------------------|
| instType | String | 否       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约<br>`instType`和`instId`同时传入的时候会校验`instId`与`instType`是否一致。 |
| instId   | String | 否       | 交易产品ID，如：`BTC-USDT-SWAP`<br>支持多个`instId`查询（不超过10个），半角逗号分隔                                                                  |
| posId   | String | 否       | 持仓ID<br>支持多个`posId`查询（不超过20个）。<br>存在有效期的属性，自最近一次完全平仓算起，满30天 posId 以及整个仓位会被清除。                                                       |

<aside class="notice">
如果该 instId 拥有过仓位且当前持仓量为0，传 instId 时，如果当前存在有效的posId，会返回仓位信息，如果当前不存在有效的 posId 时，不会返回仓位信息；不传 instId 时，仓位信息不返回。
</aside>
<aside class="notice">
逐仓交易设置中，如果设置为自主划转模式，逐仓转入保证金后，会生成一个持仓量为0的仓位
</aside>

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adl": "1",
            "availPos": "0.00190433573",
            "avgPx": "62961.4",
            "baseBal": "",
            "baseBorrowed": "",
            "baseInterest": "",
            "bePx": "",
            "bizRefId": "",
            "bizRefType": "",
            "cTime": "1724740225685",
            "ccy": "BTC",
            "clSpotInUseAmt": "",
            "closeOrderAlgo": [],
            "deltaBS": "",
            "deltaPA": "",
            "fee": "",
            "fundingFee": "",
            "gammaBS": "",
            "gammaPA": "",
            "hedgedPos": "",
            "idxPx": "62890.5",
            "imr": "",
            "instId": "BTC-USDT",
            "instType": "MARGIN",
            "interest": "0",
            "last": "62892.9",
            "lever": "5",
            "liab": "-99.9998177776581948",
            "liabCcy": "USDT",
            "liqPenalty": "",
            "liqPx": "53615.448336593756",
            "margin": "0.000317654",
            "markPx": "62891.9",
            "maxSpotInUseAmt": "",
            "mgnMode": "isolated",
            "mgnRatio": "9.404143929947395",
            "mmr": "0.0000318005395854",
            "notionalUsd": "119.756628017499",
            "optVal": "",
            "pendingCloseOrdLiabVal": "0",
            "pnl": "",
            "pos": "0.00190433573",
            "posCcy": "BTC",
            "posId": "1752810569801498626",
            "posSide": "net",
            "quoteBal": "",
            "quoteBorrowed": "",
            "quoteInterest": "",
            "realizedPnl": "",
            "spotInUseAmt": "",
            "spotInUseCcy": "",
            "thetaBS": "",
            "thetaPA": "",
            "tradeId": "785524470",
            "uTime": "1724742632153",
            "upl": "-0.0000033452492717",
            "uplLastPx": "-0.0000033199677697",
            "uplRatio": "-0.0105311101755551",
            "uplRatioLastPx": "-0.0104515220008934",
            "usdPx": "",
            "vegaBS": "",
            "vegaPA": "",
            "nonSettleAvgPx":"",
            "settledPnl":""
        }
    ],
    "msg": ""
}
```

#### 返回参数 

| 参数名                     | 类型              | 描述                                                                                                                                                                     |
|:------------------------|:----------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType                | String          | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| mgnMode                 | String          | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓                                                                                                                                   |
| posId                   | String          | 持仓ID                                                                                                                                                                   |
| posSide                 | String          | 持仓方向<br>`long`：开平仓模式开多，`pos`为正 <br>`short`：开平仓模式开空，`pos`为正<br>`net`：买卖模式（`交割`/`永续`/`期权`：`pos`为正代表开多，`pos`为负代表开空。`币币杠杆`时，`pos`均为正，`posCcy`为交易货币时，代表开多；`posCcy`为计价货币时，代表开空。） |
| pos                     | String          | 持仓量。单位：SWAP/FUTURES/OPTIONS为合约张数；MARGIN为标的币数量。符号（net模式）：正数=多头，负数=空头。long/short模式下按方向分开返回，请结合 `posSide` 判断。逐仓模式下手动划转保证金后，会生成一条 pos 为 `0` 的仓位记录（表示已划入资金但尚无持仓的状态）。 |
| hedgedPos | String | 对冲持仓数量<br>仅在delta 中性策略模式的账户返回stgyType:1，对普通策略模式的账户返回"" |
| baseBal                 | String          | ~~交易币余额，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                    |
| quoteBal                | String          | ~~计价币余额 ，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                   |
| baseBorrowed            | String          | ~~交易币已借，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                    |
| baseInterest            | String          | ~~交易币计息，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                    |
| quoteBorrowed           | String          | ~~计价币已借，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                    |
| quoteInterest           | String          | ~~计价币计息，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                                                                                                    |
| posCcy                  | String          | 仓位资产币种，仅适用于`币币杠杆`仓位                                                                                                                                                    |
| availPos                | String          | 可平仓数量，适用于 `币币杠杆`，`期权`<br>对于杠杆仓位，平仓时，杠杆还清负债后，余下的部分会视为币币交易，如果想要减少币币交易的数量，可通过"获取最大可用数量"接口获取只减仓的可用数量。                                                                      |
| avgPx                   | String          | 当前持仓的成交量加权平均开仓价格。线性合约以计价货币计价（如BTC-USDT-SWAP以USDT计），反向合约以USD计价（如BTC-USD-SWAP以USD计）。每次影响仓位大小的成交后重新计算。开仓均价<br>会随结算周期变化，特别是在交割合约全仓模式下，结算时开仓均价会更新为结算价格，同时新增头寸也会改变开仓均价。                                                                                                         |
| nonSettleAvgPx          | String          | 未结算均价<br>不受结算影响的加权开仓价格，仅在新增头寸时更新，和开仓均价的主要区别在于是否受到结算影响。<br>仅适用于`全仓``交割`                                                                                                 |
| upl                     | String          | 当前持仓按标记价格计算的未实现盈亏，以该合约的结算货币（见 `ccy`）计价。公式：线性 = (标记价格 − 开仓均价) × 持仓量 × `ctVal`；反向 = (1/开仓均价 − 1/标记价格) × 持仓量 × `ctVal`。账户层面USD总计见 GET /api/v5/account/balance 中的 `upl`。 |
| uplRatio                | String          | 未实现收益率（以标记价格计算                                                                                                                                                         |
| uplLastPx               | String          | 以最新成交价格计算的未实现收益，主要做展示使用，实际值还是 upl                                                                                                                                      |
| uplRatioLastPx          | String          | 以最新成交价格计算的未实现收益率                                                                                                                                                       |
| instId                  | String          | 产品ID，如 `BTC-USDT-SWAP`                                                                                                                                                 |
| lever                   | String          | 杠杆倍数，不适用于`期权`以及`组合保证金模式`下的全仓仓位                                                                                                                                         |
| liqPx                   | String          | 预估强平价格。这是基于当前权益和保证金率的估算值，实际强平价格可能因资金费率累计、其他仓位变动或市场剧烈波动而迅速变化。<br>不适用于 `OPTION` |
| markPx                  | String          | 最新标记价格                                                                                                                                                                 |
| imr                     | String          | 该全仓持仓的初始保证金要求，以USD计价。公式：仓位数量 × 标记价格 × 初始保证金率（1/杠杆）。账户级别IMR请见 GET /api/v5/account/balance 中的 `imr`。逐仓持仓返回空字符串。仅适用于 `全仓`。 |
| margin                  | String          | 保证金余额，可增减，仅适用于`逐仓`                                                                                                                                                     |
| mgnRatio                | String          | 维持保证金率                                                                                                                                                                   |
| mmr                     | String          | 维持保证金                                                                                                                                                                  |
| liab                    | String          | 负债额，仅适用于`币币杠杆`                                                                                                                                                         |
| liabCcy                 | String          | 负债币种，仅适用于`币币杠杆`                                                                                                                                                        |
| interest                | String          | 利息，已经生成的未扣利息                                                                                                                                                           |
| tradeId                 | String          | 最新成交ID                                                                                                                                                                 |
| optVal                  | String          | 期权市值，仅适用于`期权`                                                                                                                                                          |
| pendingCloseOrdLiabVal  | String          | 逐仓杠杆负债对应平仓挂单的数量                                                                                                                                                        |
| notionalUsd             | String          | 以美金价值为单位的持仓数量                                                                                                                                                          |
| adl                     | String          | 自动减仓（ADL）指标。范围：0–5，0 = ADL优先级最低（最不可能被强制减仓），5 = 优先级最高（保险基金耗尽时最先被减仓）。优先级随未实现盈利增大和杠杆倍数增加而升高。<br/>仅适用于 `FUTURES/SWAP/OPTION` |
| ccy                     | String          | 占用保证金的币种                                                                                                                                                               |
| last                    | String          | 最新成交价                                                                                                                                                                  |
| idxPx                   | String          | 最新指数价格                                                                                                                                                                 |
| usdPx                   | String          | 保证金币种的市场最新美金价格 仅适用于`交割`/`永续`/`期权`                                                                                                                                                |
| bePx                    | String          | 盈亏平衡价                                                                                                                                                                  |
| deltaBS                 | String          | 美金本位持仓仓位delta，仅适用于`期权`                                                                                                                                                 |
| deltaPA                 | String          | 币本位持仓仓位delta，仅适用于`期权`                                                                                                                                                  |
| gammaBS                 | String          | 美金本位持仓仓位gamma，仅适用于`期权`                                                                                                                                                 |
| gammaPA                 | String          | 币本位持仓仓位gamma，仅适用于`期权`                                                                                                                                                  |
| thetaBS                 | String          | 美金本位持仓仓位theta，仅适用于`期权`                                                                                                                                                 |
| thetaPA                 | String          | 币本位持仓仓位theta，仅适用于`期权`                                                                                                                                                  |
| vegaBS                  | String          | 美金本位持仓仓位vega，仅适用于`期权`                                                                                                                                                  |
| vegaPA                  | String          | 币本位持仓仓位vega，仅适用于`期权`                                                                                                                                                   |
| spotInUseAmt            | String          | 现货对冲占用数量<br>适用于`组合保证金模式`                                                                                                                                               |
| spotInUseCcy            | String          | 现货对冲占用币种，如 `BTC`<br>适用于`组合保证金模式`                                                                                                                                       |
| clSpotInUseAmt          | String          | 用户自定义现货占用数量<br>适用于`组合保证金模式`                                                                                                                                            |
| maxSpotInUseAmt         | String          | 系统计算得到的最大可能现货占用数量<br>适用于`组合保证金模式`                                                                                                                                      |
| realizedPnl             | String          | 已实现收益<br>仅适用于`交割`/`永续`/`期权`<br>`realizedPnl`=`pnl`+`fee`+`fundingFee`+`liqPenalty`+`settledPnl`                                                                        |
| settledPnl              | String          | 已结算收益<br>仅适用于`全仓``交割`                                                                                                                                                  |
| pnl                     | String          | 平仓订单累计收益额(不包括手续费)                                                                                                                                                              |
| fee                     | String          | 自当前仓位开仓起累计手续费，仓位完全平仓后重置为0。逐笔手续费详情请使用 GET /api/v5/trade/fills。累计手续费金额，正数代表平台返佣 ，负数代表平台扣除                                                                                                                                             |
| fundingFee              | String          | 累计资金费用                                                                                                                                                                 |
| liqPenalty              | String          | 累计爆仓罚金，有值时为负数。                                                                                                                                                         |
| closeOrderAlgo          | Array of objects | 平仓策略委托订单。调用策略委托下单，且`closeFraction`=1 时，该数组才会有值。                                                                                                                        |
| > algoId                | String          | 策略委托单ID                                                                                                                                                                |
| > slTriggerPx           | String          | 止损触发价                                                                                                                                                                  |
| > slTriggerPxType       | String          | 止损触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                  |
| > tpTriggerPx           | String          | 止盈触发价                                                                                                                                                                  |
| > tpTriggerPxType       | String          | 止盈触发价类型<br>`last`：最新价格<br>`index`：指数价格<br>`mark`：标记价格                                                                                                                  |
| > closeFraction         | String          | 策略委托触发时，平仓的百分比。1 代表100%                                                                                                                                                |
| cTime                   | String          | 持仓创建时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                                 |
| uTime                   | String          | 最近一次持仓更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                             |
| bizRefId                | String          | 外部业务id，如 体验券id                                                                                                                                                         |
| bizRefType              | String          | 外部业务类型                                                                                                                                                                 |

<aside class="notice">
PM账户下，持仓的 IMR MMR的数据是后端服务以ristUnit为最小粒度重新计算，相同riskUnit全仓仓位的imr和mmr返回值相同。
</aside>
