---
site: us
lang: zh
name: 查看账户余额
method: GET
path: /api/v5/account/balance
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 查看账户余额 [trading-account-rest-api-get-balance]

获取交易账户中资金余额信息。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/balance`

> 请求示例

```shell
# 获取账户中所有资产余额
GET /api/v5/account/balance

# 获取账户中BTC、ETH两种资产余额
GET /api/v5/account/balance?ccy=BTC,ETH

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户余额
result = accountAPI.get_account_balance()
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| ccy    | String | 否       | 币种，如 `BTC`<br>支持多币种查询（不超过20个），币种之间半角逗号分隔 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "55415.624719833286",
            "availEq": "",
            "borrowFroz": "0",
            "delta": "0",
            "deltaLever": "0",
            "deltaNeutralStatus": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "availBal": "4834.317093622894",
                    "availEq": "4834.3170936228935",
                    "borrowFroz": "0",
                    "cashBal": "4850.435693622894",
                    "ccy": "USDT",
                    "crossLiab": "0",
                    "colRes": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
        			"colBorrAutoConversion": "0",
                    "disEq": "4991.542013297616",
                    "eq": "4992.890093622894",
                    "eqUsd": "4991.542013297616",
                    "smtSyncEq": "0",
                    "spotCopyTradingEq": "0",
                    "fixedBal": "0",
                    "frozenBal": "158.573",
                    "frpType": "0",
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
                    "maxSpotInUse": "",
                    "spotIsoBal": "0",
                    "stgyEq": "150",
                    "twap": "0",
                    "uTime": "1705449605015",
                    "upl": "-7.545600000000006",
                    "uplLiab": "0",
                    "spotBal": "",
                    "openAvgPx": "",
                    "accAvgPx": "",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "totalPnl": "",
                    "totalPnlRatio": ""
                }
            ],
            "imr": "0",
            "isoEq": "0",
            "mgnRatio": "",
            "mmr": "0",
            "notionalUsd": "0",
            "notionalUsdForBorrow": "0",
            "notionalUsdForFutures": "0",
            "notionalUsdForOption": "0",
            "notionalUsdForSwap": "0",
            "ordFroz": "",
            "totalEq": "55837.43556134779",
            "uTime": "1705474164160",
            "upl": "0",
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型**   | **描述**                                                                                 |
|-----------------------|----------|----------------------------------------------------------------------------------------|
| uTime                 | String   | 账户信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                              |
| totalEq               | String   | 美金层面权益                                                                                 |
| isoEq                 | String   | 美金层面逐仓仓位权益<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                        |
| adjEq                 | String   | 美金层面有效保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                            |
| availEq | String | 账户美金层面可用保证金，排除因总质押借币上限而被限制的币种<br> 适用于`跨币种保证金模式/组合保证金模式` |
| ordFroz               | String   | 美金层面全仓挂单占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`                                                        |
| imr                   | String   | 美金层面占用保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| mmr                   | String   | 美金层面维持保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| borrowFroz            | String   | 账户美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                               |
| mgnRatio              | String   | 美金层面维持保证金率<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                    |
| notionalUsd           | String   | 以美金价值为单位的持仓数量，即仓位美金价值<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                       |
| notionalUsdForBorrow  | String   | 借币金额（美元价值）<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| notionalUsdForSwap    | String   | 永续合约持仓美元价值<br>适用于`跨币种保证金模式`/`组合保证金模式`        |
| notionalUsdForFutures | String   | 交割合约持仓美元价值<br>适用于`跨币种保证金模式`/`组合保证金模式`        |
| notionalUsdForOption  | String   | 期权持仓美元价值<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                     |
| upl                   | String   | 账户层面全仓未实现盈亏（美元单位）<br>适用于`跨币种保证金模式`/`组合保证金模式`                                           |
| delta | String | Delta (USD) |
| deltaLever | String | Delta权益比率<br>deltaLever = delta/totalEq |
| deltaNeutralStatus | String | Delta 风险状态<br>`0`: 普通<br>`1`: 限制划转<br>`2`: 仅支持降低 Delta - 相同基础货币的现货、交割和永续合约视为同一标的资产。同一标的资产内，仅能新下一笔降低 Delta 值的订单，且下单时不应存在其他挂单。如果触发此限制，且您的账户 Delta 大于 500,000 USD，您的所有限价、市价、高级限价单挂单将被撤销。 |
| details               | Array of objects    | 各币种资产详细信息                                                                              |
| > ccy                 | String   | 币种                                                                                     |
| > eq                  | String   | 币种总权益                                                                                  |
| > cashBal             | String   | 币种余额                                                                                   |
| > uTime               | String   | 币种余额信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                            |
| > isoEq               | String   | 币种逐仓仓位权益<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                          |
| > availEq             | String   | 可用保证金<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                             |
| > disEq               | String   | 美金层面币种折算权益<br>适用于`现货模式`(开通了借币功能)/`跨币种保证金模式`/`组合保证金模式`                                                                             |
| > fixedBal            | String   | 抄底宝、逃顶宝功能的币种冻结金额                                                                       |
| > availBal            | String   | 可用余额                                                                                   |
| > frozenBal           | String   | 币种占用金额                                                                                 |
| > ordFrozen           | String   | 挂单冻结数量 <br>适用于`现货模式`/`合约模式`/`跨币种保证金模式`                                              |
| > liab                | String   | 币种负债额<br>值为正数，如 "21625.64"<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                  |
| > upl                 | String   | 未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                             |
| > uplLiab             | String   | 由于仓位未实现亏损导致的负债<br>适用于`跨币种保证金模式`/`组合保证金模式`                                              |
| > crossLiab           | String   | 币种全仓负债额<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                     |
| > isoLiab             | String   | 币种逐仓负债额<br>适用于`跨币种保证金模式`/`组合保证金模式`                                                     |
| > mgnRatio            | String   | 币种全仓维持保证金率，衡量账户内某项资产风险的指标<br>适用于`合约模式`且有全仓仓位时                                         |
| > imr                 | String   | 币种维度全仓占用保证金<br>适用于`合约模式`且有全仓仓位时                                                     |
| > mmr                 | String   | 币种维度全仓维持保证金<br>适用于`合约模式`且有全仓仓位时                                                     |
| > interest            | String   | 计息，应扣未扣利息<br>值为正数，如 `9.01`<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                  |
| > twap                | String   | 当前负债币种触发自动换币的风险<br>0、1、2、3、4、5其中之一，数字越大代表您的负债币种触发自动换币概率越高<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| > frpType | String | 自动换币类型<br> `0`：未发生自动换币<br> `1`：基于用户的自动换币<br> `2`：基于平台借币限额的自动换币<br><br> 当twap>=1时返回1或2代表自动换币风险类型，适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| > maxLoan             | String   | 币种最大可借<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` 的全仓                                                  |
| > eqUsd               | String   | 币种权益美金价值                                                                               |
| > borrowFroz          | String   | 币种美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                               |
| > notionalLever       | String   | 币种杠杆倍数<br>适用于`合约模式`                                                                 |
| > stgyEq              | String   | 策略权益                                                                                   |
| > isoUpl              | String   | 逐仓未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                           |
| > spotInUseAmt        | String   | 现货对冲占用数量<br>适用于`组合保证金模式`                                                               |
| > spotIsoBal          | String   | 现货逐仓余额<br>仅适用于现货带单/跟单<br>适用于`现货模式`/`合约模式`                                           |
| > spotBal             | String   | 现货余额 ，单位为 币种，比如 BTC。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)  |
| > openAvgPx           | String    | 现货开仓成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > accAvgPx            | String    | 现货累计成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > spotUpl             | String   | 现货未实现收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > spotUplRatio        | String   | 现货未实现收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)             |
| > totalPnl            | String   | 现货累计收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)       |
| > totalPnlRatio       | String   | 现货累计收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)              |
| > colRes               | String          | 平台维度质押限制状态<br> `0`：限制未触发<br> `1`：限制未触发，但该币种接近平台质押上限<br> `2`：限制已触发。该币种不可用作新订单的保证金，这可能会导致下单失败。但它仍会被计入账户有效保证金，保证金率不会收到影响。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| > colBorrAutoConversion | String 		| 基于平台质押借币限额的自动换币风险指标。分为1-5多个等级，数字越大，触发自动换币的可能性越大。默认值为0，表示当前无风险。5表示该用户正在进行自动换币，4代表该用户即将被进行自动换币，1/2/3表示存在自动换币风险。<br> 适用于`现货模式`/`合约模式`/`跨币种保证金模式`/`组合保证金模式`<br> 当某币种的全平台质押借币量超出平台总上限一定比例时，对于质押该币种且借币量较大的用户，平台将通过自动换币降低质押借币风险。请减少该币种的质押数量或偿还负债，以降低风险。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| > collateralRestrict | Boolean | ~~平台维度的质押借币限制<br> `true`<br> `false`~~（已弃用，请使用colRes） |
| > collateralEnabled   | Boolean   | `true`：质押币<br>`false`：非质押币<br>适用于`跨币种保证金模式 |
| > autoLendStatus | String | 自动借出状态<br> `unsupported`：该币种不支持自动借出<br> `off`：自动借出功能关闭<br>`pending`：自动借出功能开启但未匹配<br>`active`：自动借出功能开启且已匹配 |
| > autoLendMtAmt | String | 自动借出已匹配量<br> 当 autoLendStatus 为 `unsupported/off/pending` 时返回 0<br> 当 autoLendStatus 为 `active` 时返回已匹配量 |

<aside class="notice">
当前账户等级下无效字段返回""
</aside>

<aside class="notice">
cashBal 和 eq 同时为 0 的币种过滤不返回
</aside>

各账户等级下有效字段分布

| 参数                    | 现货模式 | 合约模式 | 跨币种保证金模式 | 组合保证金模式 |
|:----------------------|:-----|:--------|:---------|:--------|
| uTime                 | 是    | 是       | 是        | 是       |
| totalEq               | 是    | 是       | 是        | 是       |
| isoEq                 |      | 是       | 是        | 是       |
| adjEq                 | 是    |         | 是        | 是       | 
| availEq                |     |         | 是        | 是           | 
| ordFroz               | 是    |         | 是        | 是       |
| imr                   | 是    |         | 是        | 是       |
| mmr                   | 是    |         | 是        | 是       |
| borrowFroz            | 是    |         | 是        | 是       |
| mgnRatio              | 是    |         | 是        | 是       |
| notionalUsd           | 是    |         | 是        | 是       |
| notionalUsdForSwap    |      |         | 是        | 是       |
| notionalUsdForFutures |      |         | 是        | 是       |
| notionalUsdForOption  | 是    |         | 是        | 是       |
| notionalUsdForBorrow  | 是    |         | 是        | 是       |
| upl                   |      |         | 是        | 是       |
| details               |      |         |          |         |
| > ccy                 | 是    | 是       | 是        | 是       |
| > eq                  | 是    | 是       | 是        | 是       |
| > cashBal             | 是    | 是       | 是        | 是       |
| > uTime               | 是    | 是       | 是        | 是       |
| > isoEq               |      | 是       | 是        | 是       |
| > availEq             |      | 是       | 是        | 是       |
| > disEq               | 是    |         | 是        | 是       |
| > availBal            | 是    | 是       | 是        | 是       |
| > frozenBal           | 是    | 是       | 是        | 是       |
| > ordFrozen           | 是    | 是       | 是        | 是       |
| > liab                | 是    |         | 是        | 是       |
| > upl                 |      | 是       | 是        | 是       |
| > uplLiab             |      |         | 是        | 是       |
| > crossLiab           | 是    |         | 是        | 是       |
| > isoLiab             |      |         | 是        | 是       |
| > mgnRatio            |      | 是       |          |         |
| > interest            | 是    |         | 是        | 是       |
| > twap                | 是    |         | 是        | 是       |
| > maxLoan             | 是    |         | 是        | 是       |
| > eqUsd               | 是    | 是       | 是        | 是       |
| > borrowFroz          | 是    |         | 是        | 是       |
| > notionalLever       |      | 是       |          |         |
| > stgyEq              | 是    | 是       | 是        | 是       |
| > isoUpl              |      | 是       | 是        | 是       |
| > spotInUseAmt        |      |         |          | 是        |
| > spotIsoBal          | 是    | 是       |          |         |
| > imr                 |      | 是       |          |         |
| > mmr                 |      | 是       |          |         |
| > spotBal             | 是    | 是       | 是                            | 是            |
| > openAvgPx           | 是    | 是       | 是                            | 是            |
| > accAvgPx            | 是    | 是       | 是                            | 是            |
| > spotUpl             | 是    | 是       | 是                            | 是            |
| > spotUplRatio        | 是    | 是       | 是                            | 是            |
| > totalPnl            | 是    | 是       | 是                            | 是            |
| > totalPnlRatio       | 是    | 是       | 是                            | 是            |
