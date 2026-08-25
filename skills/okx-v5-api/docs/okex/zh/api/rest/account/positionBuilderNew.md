---
site: okex
lang: zh
name: 仓位创建器
method: POST
path: /api/v5/account/position-builder
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 仓位创建器 [trading-account-rest-api-position-builder-new]

计算用户的模拟头寸或当前头寸的投资组合保证金信息，一次请求最多可添加200个虚拟仓位和200个虚拟虚拟资产

#### 限速：2次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/position-builder`

> 请求示例

```shell
# 真实与虚拟的仓位与资产一起计算
POST /api/v5/account/position-builder
body
{
    "inclRealPosAndEq": false,
    "simPos":[
         {
            "pos":"-10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000"
         },
         {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
         }
    ],
    "simAsset":[
        {
            "ccy": "USDT",
            "amt": "100"
        }
    ],
    "greeksType":"CASH"
}

# 只计算已有真实仓位
POST /api/v5/account/position-builder
body
{
   "inclRealPosAndEq": true
}

# 只计算虚拟仓位
POST /api/v5/account/position-builder
body
{
    "acctLv": "4",
    "inclRealPosAndEq": false,
    "simPos":[
        {
            "pos":"10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000"
        },
        {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
        }
    ]
}

# 切换到跨币种
POST /api/v5/account/position-builder
body
{
    "acctLv": "3",
    "lever":"10",
    "simPos":[
        {
            "pos":"10",
            "instId":"BTC-USDT-SWAP",
            "avgPx":"100000",
            "lever":"5"
        },
        {
            "pos":"10",
            "instId":"LTC-USDT-SWAP",
            "avgPx":"8000"
        }
    ]
}
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

result = accountAPI.position_builder(
    inclRealPosAndEq=True,
    simPos=[
        {
            "pos": "10",
            "instId": "BTC-USDT-SWAP",
            "avgPx":"100000"
        },
        {
            "pos": "10",
            "instId": "LTC-USDT-SWAP",
            "avgPx":"100000"
        }
    ]
)
print(result)
```

#### 请求参数

| 参数名                | 类型              | 是否必须 | 描述                                                                 |
|:-------------------|:----------------|:-----|:-------------------------------------------------------------------|
| acctLv             | String         | 否    | 切换至账户模式<br>`3`：跨币种保证金模式<br>`4`：组合保证金模式                             |
| inclRealPosAndEq   | Boolean         | 否    | 是否代入已有仓位和资产<br>默认为`true`                                           |
| lever              | String          | 否    | 跨币种下整体的全仓合约杠杆数量，默认为`1`。<br>如果超过允许的杠杆倍数，按照最大的杠杆设置。<br>适用于`跨币种保证金模式` |
| simPos             | Array of objects | 否    | 模拟仓位列表                                                             |
| > instId           | String          | 是    | 交易产品ID，如 `BTC-USDT-SWAP`<br>适用于 `SWAP`/`FUTURES`/`OPTION`          |
| > pos              | String          | 是    | 持仓量                                                                |
| > avgPx            | String          | 是    | 平均开仓价格                                                             |
| > lever            | String          | 否    | 杠杆<br>仅适用于在跨币种保证金模式下指定交易产品的杠杆。如果用户不传，则选择默认杠杆为`1`。                  |
| simAsset         | Array of objects | 否    | 模拟资产<br>当`inclRealPosAndEq`为`true`，只考虑真实资产，会忽略虚拟资产                 |
| > ccy            | String          | 是    | 币种，如 `BTC`                                                         |
| > amt            | String          | 是    | 币种数量<br>可以为负，代表减少币种资产                                              |
| greeksType       | String          | 否    | 希腊值类型<br>`BS`：BS模型<br>`PA`：币本位<br>`CASH`：美元现金等价<br>默认是`BS`         |
| idxVol | String | 否 | 价格变动百分比。小数形式，范围 -0.99 ~ 1，以 0.01 为增量。<br> 默认值为 0 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "acctLever": "-0.1364949794742562",
            "assets": [
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "BTC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "LTC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "0",
                    "borrowImr": "0",
                    "borrowMmr": "",
                    "ccy": "USDC",
                    "spotInUse": "0"
                },
                {
                    "availEq": "-78589.37",
                    "borrowImr": "7855.32188898",
                    "borrowMmr": "",
                    "ccy": "USDT",
                    "spotInUse": "0"
                }
            ],
            "borrowMmr": "1571.064377796",
            "derivMmr": "1375.4837063088003",
            "eq": "-78553.21888979999",
            "marginRatio": "-25.95365779811705",
            "positions": [],
            "riskUnitData": [
                {
                    "delta": "-9704.903689800001",
                    "gamma": "0",
                    "imrBf": "",
                    "imr": "1538.9669514070802",
                    "mmrBf": "",
                    "mmr": "1183.8207318516002",
                    "mr1": "1164.4109244719994",
                    "mr1FinalResult": {
                        "pnl": "-1164.4109244719994",
                        "spotShock": "0.12",
                        "volShock": "up"
                    },
                    "mr1Scenarios": {
                        "volSame": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        },
                        "volShockDown": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        },
                        "volShockUp": {
                            "0": "0",
                            "0.08": "-776.2739496480004",
                            "-0.08": "776.2739496480004",
                            "0.04": "-388.1369748240002",
                            "0.12": "-1164.4109244719994",
                            "-0.12": "1164.4109244719994",
                            "-0.04": "388.1369748240002"
                        }
                    },
                    "mr2": "0",
                    "mr3": "0",
                    "mr4": "19.4098073796",
                    "mr5": "0",
                    "mr6": "1164.4109244720003",
                    "mr6FinalResult": {
                        "pnl": "-2328.8218489440005",
                        "spotShock": "0.24"
                    },
                    "mr7": "43.67206660410001",
                    "mr8": "1571.064377796",
                    "mr9": "0",
                    "portfolios": [
                        {
                            "amt": "-10",
                            "avgPx": "100000",
                            "delta": "-9704.903689800001",
                            "floatPnl": "290.6300000000003",
                            "gamma": "0",
                            "instId": "BTC-USDT-SWAP",
                            "instType": "SWAP",
                            "isRealPos": false,
                            "markPxBf": "",
                            "markPx": "97093.7",
                            "notionalUsd": "9703.22",
                            "posSide": "net",
                            "theta": "0",
                            "vega": "0"
                        }
                    ],
                    "riskUnit": "BTC",
                    "theta": "0",
                    "upl": "290.49631020000027",
                    "vega": "0"
                },
                {
                    "delta": "1019.5308",
                    "gamma": "0",
                    "imrBf": "",
                    "imr": "249.16186679436",
                    "mmrBf": "",
                    "mmr": "191.6629744572",
                    "mr1": "183.50672805719995",
                    "mr1FinalResult": {
                        "pnl": "-183.50672805719995",
                        "spotShock": "-0.18",
                        "volShock": "up"
                    },
                    "mr1Scenarios": {
                        "volSame": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        },
                        "volShockDown": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        },
                        "volShockUp": {
                            "0": "0",
                            "-0.06": "-61.168909352399936",
                            "0.06": "61.168909352399936",
                            "-0.18": "-183.50672805719995",
                            "0.18": "183.50672805719995",
                            "0.12": "122.33781870480001",
                            "-0.12": "-122.33781870480001"
                        }
                    },
                    "mr2": "0",
                    "mr3": "0",
                    "mr4": "8.1562464",
                    "mr5": "0",
                    "mr6": "183.5067280572",
                    "mr6FinalResult": {
                        "pnl": "-367.0134561144",
                        "spotShock": "-0.36"
                    },
                    "mr7": "7.1367156",
                    "mr8": "1571.064377796",
                    "mr9": "0",
                    "portfolios": [
                        {
                            "amt": "10",
                            "avgPx": "8000",
                            "delta": "1019.5308",
                            "floatPnl": "-78980",
                            "gamma": "0",
                            "instId": "LTC-USDT-SWAP",
                            "instType": "SWAP",
                            "isRealPos": false,
                            "markPxBf": "",
                            "markPx": "102",
                            "notionalUsd": "1018.9",
                            "posSide": "net",
                            "theta": "0",
                            "vega": "0"
                        }
                    ],
                    "riskUnit": "LTC",
                    "theta": "0",
                    "upl": "-78943.6692",
                    "vega": "0"
                }
            ],
            "totalImr": "9643.45070718144",
            "totalMmr": "2946.5480841048",
            "ts": "1736936801642",
            "upl": "-78653.1728898"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名              | 类型              | 描述                                                                                                                                                           |
|:-----------------|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eq               | String          | 账户有效保证金                                                                                                                                                      |
| totalMmr         | String          | 账户维持保证金，单位为`USD`                                                                                                                                             |
| totalImr         | String          | 账户初始保证金占用，单位为`USD`                                                                                                                                           |
| borrowMmr        | String          | 账户借币维持保证金，单位为`USD`                                                                                                                                           |
| derivMmr         | String          | 账户衍生品维持保证金，单位为`USD`                                                                                                                                          |
| marginRatio      | String          | 账户全仓维持保证金率                                                                                                                                                     |
| upl              | String          | 账户浮动盈亏                                                                                                                                                       |
| acctLever        | String          | 账户全仓杠杆                                                                                                                                                       |
| ts               | String          | 账户信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                                                                                    |
| assets           | Array of objects | 资产信息                                                                                                                                                         |
| > ccy            | String          | 币种，如 `BTC`                                                                                                                                                   |
| > availEq        | String          | 币种权益                                                                                                                                                         |
| > spotInUse      | String          | 现货对冲占用                                                                                                                                                       |
| > borrowMmr      | String          | <del>借币维持保证金，单位为`USD`</del>字段已废弃                                                                                                                             |
| > borrowImr      | String          | 借币初始保证金，单位为`USD`                                                                                                                                             |
| riskUnitData     | Array of objects | Risk unit 相关信息<br>适用于`组合保证金模式`                                                                                                                                               |
| > riskUnit       | String          | 账户内的 risk unit，如 `BTC`                                                                                                                                       |
| > mmrBf | String | 价格变动前 Risk unit 维度的维持保证金，单位为`USD`<br> 若用户没有传入idxVol，则返回 "" |
| > mmr            | String          | Risk unit 维度的维持保证金，单位为`USD`                                                                                                                                  |
| > imrBf | String | 价格变动前 Risk unit 维度的初始保证金，单位为`USD`<br> 若用户没有传入idxVol，则返回 "" |
| > imr            | String          | Risk unit 维度的初始保证金，单位为`USD`                                                                                                                                  |
| > upl            | String          | Risk unit 维度的浮动盈亏，单位为`USD`                                                                                                                                   |
| > mr1            | String          | 现货和波动率变化风险 (适用于所有衍生品，以及在现货对冲模式下的现货)                                                                                                                          |
| > mr2            | String          | 时间价值风险 (仅适用于期权)                                                                                                                                              |
| > mr3            | String          | 波动率跨期风险 (仅适用于期权)                                                                                                                                             |
| > mr4            | String          | 基差风险 (适用于所有衍生品)                                                                                                                                              |
| > mr5            | String          | 利率风险 (仅适用于期权)                                                                                                                                                |
| > mr6            | String          | 极端市场波动风险 (适用于所有衍生品，以及在现货对冲模式下的现货)                                                                                                                            |
| > mr7            | String          | 减仓成本 (适用于所有衍生品)                                                                                                                                              |
| > mr8            | String          | 借币维持保证金/初始保证金                                                                                                                                                |
| > mr9            | String          | USDT-USDC-USD 对冲风险                                                                                                                                           |
| > mr1Scenarios   | Object of objects          | MR1 的压力测试场景分析                                                                                                                                                |
| >> volShockDown  | Object          | 波动率向下时，不同价格波动比率下的压力测试盈亏<br>值为 {`change`: `value`, ...} <br>`change`：价格波动比率（百分比），如 `0.01` 代表 `1%`<br>`value`：压力测试下的盈亏，单位为`USD`<br>如 {"-0.15":"-2333.23", ...} |
| >> volSame       | Object          | 波动率不变时，不同价格波动比率下的压力测试盈亏<br>值为 {`change`: `value`, ...} <br>`change`：价格波动比率（百分比），如 `0.01` 代表 `1%`<br>`value`：压力测试下的盈亏，单位为`USD`<br>如 {"-0.15":"-2333.23", ...} |
| >> volShockUp    | Object          | 波动率向上时，不同价格波动比率下的压力测试盈亏<br>值为 {`change`: `value`, ...} <br>`change`：价格波动比率（百分比），如 `0.01` 代表 `1%`<br>`value`：压力测试下的盈亏，单位为`USD`<br>如 {"-0.15":"-2333.23", ...} |
| > mr1FinalResult | Object          | MR1 最大亏损场景                                                                                                                                                   |
| >> pnl           | String          | MR1 最大亏损压测盈亏，单位为 `USD`                                                                                                                                       |
| >> spotShock     | String          | MR1 最大亏损的价格波动（百分比），如 `0.01` 代表 `1%`                                                                                                                          |
| >> volShock      | String          | MR1 最大亏损波动率趋势<br>`down`：波动率向下<br>`unchange`：波动率不变<br>`up`：波动率向上                                                                                              |
| > mr6FinalResult | Object          | MR6 最大亏损场景                                                                                                                                                   |
| >> pnl           | String          | MR6 最大亏损压测盈亏，单位为 `USD`                                                                                                                                       |
| >> spotShock     | String          | MR6 最大亏损的价格波动（百分比），如 `0.01` 代表 `1%`                                                                                                                          |
| > delta          | String          | (Risk unit 维度) 合约价格随标的价格变动的比例<br>当标的价格变动 x 时，合约价格变动约为此 Delta 数值乘以 x                                                                                          |
| > gamma          | String          | (Risk unit 维度) 标的价格对 Delta 值的影响程度<br>当标的价格变动 x% 时，期权 Delta 值的变动约为此 Gamma 数值乘以 x%                                                                             |
| > theta          | String          | (Risk unit 维度) 距离到期日时间缩短 1 天，该合约价格的变化量                                                                                                                       |
| > vega           | String          | (Risk unit 维度) 标的波动率增加 1%，该合约价格的变化量                                                                                                                          |
| > portfolios     | Array of objects | 资产组合                                                                                                                                                         |
| >> instId        | String          | 产品ID，如 `BTC-USDT-SWAP`                                                                                                                                       |
| >> instType      | String          | 产品类型<br>`SPOT`：现货<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权                                                                                            |
| >> amt           | String          | `instType`为`SPOT`，代表现货对冲占用<br>`instType`为`SWAP`/`FUTURES`/`OPTION`，代表仓位数量。                                                                                   |
| >> posSide       | String          | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                                                                                      |
| >> avgPx         | String          | 平均开仓价格                                                                                                                                                       |
| >> markPxBf | String | 价格变动前标记价格<br> 若用户没有传入idxVol，则返回 "" |
| >> markPx        | String          | 标记价格                                                                                                                                                         |
| >> floatPnl      | String          | 浮动盈亏                                                                                                                                                         |
| >> notionalUsd   | String          | 美金价值                                                                                                                                                         |
| >> delta         | String          | `instType`为`SPOT`，代表资产数量。<br>`instType`为`SWAP`/`FUTURES`/`OPTION`，代表(产品层面) 合约价格随标的价格变动的比例。                                                                   |
| >> gamma         | String          | (产品层面) 标的价格对 Delta 值的影响程度<br>`instType`为`SPOT`，返回""                                                                                                          |
| >> theta         | String          | (产品层面) 距离到期日时间缩短 1 天，该合约价格的变化量<br>`instType`为`SPOT`，返回""                                                                                                     |
| >> vega          | String          | (产品层面) 标的波动率增加 1%，该合约价格的变化量<br>`instType`为`SPOT`，返回""                                                                                                        |
| >> isRealPos     | Boolean         | 是否为真实仓位<br>`instType`为`SWAP`/`FUTURES`/`OPTION`，该字段有效，其他都默认返回`false`                                                                                         |
| positions        | Array of objects | 仓位信息<br>适用于`跨币种保证金模式`                                                                                                                                          |
| > instId         | String          | 产品ID，如 `BTC-USDT-SWAP`                                                                                                                                       |
| > instType       | String          | 产品类型<br>`SPOT`：现货<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权                                                                                            |
| > amt            | String          | `instType`为`SPOT`，代表现货对冲占用<br>`instType`为`SWAP`/`FUTURES`/`OPTION`，代表仓位数量。                                                                                   |
| > posSide        | String          | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                                                                                      |
| > avgPx          | String          | 平均开仓价格                                                                                                                                                       |
| > markPxBf | String | 价格变动前标记价格<br> 若用户没有传入idxVol，则返回 "" |
| > markPx         | String          | 标记价格                                                                                                                                                         |
| > floatPnl       | String          | 浮动盈亏                                                                                                                                                         |
| > imrBf | String | 价格变动前初始保证金 |
| > imr            | String          | 初始保证金，仅适用于全仓                                                                                                                                                 |
| > mgnRatio       | String          | 维持保证金率                                                                                                                                                         |
| > lever          | String          | 杠杆倍数                                                                                                                                                         |
| > notionalUsd    | String          | 美金价值                                                                                                                                                         |
| > isRealPos      | Boolean         | 是否为真实仓位<br>`instType`为`SWAP`/`FUTURES`/`OPTION`，该字段有效，其他都默认返回`false`                                                                                         |
