---
site: us
lang: zh
name: 获取子账户交易账户余额
method: GET
path: /api/v5/account/subaccount/balances
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取子账户交易账户余额 [sub-account-rest-api-get-sub-account-trading-balance]

获取子账户交易账户余额（适用于母账户）

#### 限速：6次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/account/subaccount/balances`

> 请求示例

```shell
GET /api/v5/account/subaccount/balances?subAcct=test1

```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取子账户交易账户余额
result = subAccountAPI.get_account_balance(
    subAcct="hahawang1"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| subAcct | String | 是        | 子账户名称   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "adjEq": "101.46752000000001",
            "availEq": "",
            "borrowFroz": "0",
            "delta": "0",
            "deltaLever": "0",
            "deltaNeutralStatus": "0",
            "details": [
                {
                    "autoLendStatus": "off",
                    "autoLendMtAmt": "0",
                    "accAvgPx": "",
                    "availBal": "101.5",
                    "availEq": "101.5",
                    "borrowFroz": "0",
                    "cashBal": "101.5",
                    "ccy": "USDT",
                    "clSpotInUseAmt": "",
                    "crossLiab": "0",
                    "colRes": "0",
                    "collateralEnabled": false,
                    "collateralRestrict": false,
        			"colBorrAutoConversion": "0",
                    "disEq": "101.46752000000001",
                    "eq": "101.5",
                    "eqUsd": "101.46752000000001",
                    "fixedBal": "0",
                    "frozenBal": "0",
                    "frpType": "0",
                    "imr": "",
                    "interest": "0",
                    "isoEq": "0",
                    "isoLiab": "0",
                    "isoUpl": "0",
                    "liab": "0",
                    "maxLoan": "1015.0000000000001",
                    "maxSpotInUse": "",
                    "mgnRatio": "",
                    "mmr": "",
                    "notionalLever": "",
                    "openAvgPx": "",
                    "ordFrozen": "0",
                    "rewardBal": "",
                    "smtSyncEq": "0",
                    "spotBal": "",
                    "spotCopyTradingEq": "0",
                    "spotInUseAmt": "",
                    "spotIsoBal": "0",
                    "spotUpl": "",
                    "spotUplRatio": "",
                    "stgyEq": "0",
                    "totalPnl": "",
                    "totalPnlRatio": "",
                    "twap": "0",
                    "uTime": "1663854334734",
                    "upl": "0",
                    "uplLiab": "0"
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
            "ordFroz": "0",
            "totalEq": "101.46752000000001",
            "uTime": "1739332269934",
            "upl": "0"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型**   | **描述**                                                                                             |
|-----------------------|----------|----------------------------------------------------------------------------------------------------|
| uTime                 | String   | 账户信息的更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                              |
| totalEq               | String   | 美金层面权益                                                                                 |
| isoEq                 | String   | 美金层面逐仓仓位权益<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                        |
| adjEq                 | String   | 美金层面有效保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                            |
| availEq | String | 账户美金层面可用保证金，排除因总质押借币上限而被限制的币种<br> 适用于`跨币种保证金模式/组合保证金模式` |
| ordFroz               | String   | 美金层面全仓挂单占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                          |
| imr                   | String   | 美金层面占用保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                  |
| mmr                   | String   | 美金层面维持保证金<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                  |
| borrowFroz            | String   | 账户美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                              |
| mgnRatio              | String   | 美金层面维持保证金率<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                   |
| notionalUsd           | String   | 以美金价值为单位的持仓数量，即仓位美金价值<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                      |
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
| > disEq               | String   | 美金层面币种折算权益                                                                             |
| > fixedBal            | String   | 抄底宝、逃顶宝功能的币种冻结金额                                                                       |
| > availBal            | String   | 可用余额                                                                                   |
| > frozenBal           | String   | 币种占用金额                                                                                 |
| > ordFrozen           | String   | 挂单冻结数量 <br>适用于`现货模式`/`合约模式`/`跨币种保证金模式`                                              |
| > liab                | String   | 币种负债额<br>值为正数，如 "21625.64"<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                 |
| > upl                 | String   | 未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                             |
| > uplLiab             | String   | 由于仓位未实现亏损导致的负债<br>适用于`跨币种保证金模式`/`组合保证金模式`                                              |
| > crossLiab           | String   | 币种全仓负债额<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                                    |
| > isoLiab             | String   | 币种逐仓负债额<br>适用于`跨币种保证金模式`/`组合保证金模式`                                                     |
| > rewardBal           | String   | 体验金余额                                                                                              |
| > mgnRatio            | String   | 币种全仓维持保证金率，衡量账户内某项资产风险的指标<br>适用于`合约模式`且有全仓仓位时                                         |
| > imr                 | String   | 币种维度全仓占用保证金<br>适用于`合约模式`且有全仓仓位时                                                     |
| > mmr                 | String   | 币种维度全仓维持保证金<br>适用于`合约模式`且有全仓仓位时                                                     |
| > interest            | String   | 计息，应扣未扣利息<br>值为正数，如 `9.01`<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`                                 |
| > twap                | String   | 当前负债币种触发自动换币的风险<br>0、1、2、3、4、5其中之一，数字越大代表您的负债币种触发自动换币概率越高<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| > frpType | String | 自动换币类型<br> `0`：未发生自动换币<br> `1`：基于用户的自动换币<br> `2`：基于平台借币限额的自动换币<br><br> 当twap>=1时返回1或2代表自动换币风险类型，适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` |
| > maxLoan             | String   | 币种最大可借<br>适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式` 的全仓                                                 |
| > eqUsd               | String   | 币种权益美金价值                                                                               |
| > borrowFroz          | String   | 币种美金层面潜在借币占用保证金<br>仅适用于`现货模式`/`跨币种保证金模式`/`组合保证金模式`。在其他账户模式下为""。                              |
| > notionalLever       | String   | 币种杠杆倍数<br>适用于`合约模式`                                                                 |
| > stgyEq              | String   | 策略权益                                                                                   |
| > isoUpl              | String   | 逐仓未实现盈亏<br>适用于`合约模式`/`跨币种保证金模式`/`组合保证金模式`                                           |
| > spotInUseAmt        | String   | 现货对冲占用数量<br>适用于`组合保证金模式`                                                               |
| > clSpotInUseAmt      | String   | 用户自定义现货占用数量<br>适用于`组合保证金模式`                                                                        |
| > maxSpotInUse        | String   | 系统计算得到的最大可能现货占用数量<br>适用于`组合保证金模式`                                                                  |
| > spotIsoBal          | String   | 现货逐仓余额<br>仅适用于现货带单/跟单<br>适用于`现货模式`/`合约模式`                                           |
| > smtSyncEq           | String   | 合约智能跟单权益<br>默认为0，仅适用于跟单人。                                                                          |
| > spotCopyTradingEq   | String   | 现货智能跟单权益<br>默认为0，仅适用于跟单人。                                                                          |
| > spotBal             | String   | 现货余额 ，单位为 币种，比如 BTC。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)  |
| > openAvgPx           | String   | 现货开仓成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > accAvgPx            | String   | 现货累计成本价 单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > spotUpl             | String   | 现货未实现收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)      |
| > spotUplRatio        | String   | 现货未实现收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)             |
| > totalPnl            | String   | 现货累计收益，单位 USD。 [详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)       |
| > totalPnlRatio       | String   | 现货累计收益率。[详情](https://www.okx.com/zh-hans/help/i-introduction-of-spot)              |
| > colRes               | String          | 平台维度质押限制状态<br> `0`：限制未触发<br> `1`：限制未触发，但该币种接近平台质押上限<br> `2`：限制已触发。该币种不可用作新订单的保证金，这可能会导致下单失败。但它仍会被计入账户有效保证金，保证金率不会收到影响。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| > colBorrAutoConversion | String 		| 基于平台质押借币限额的自动换币风险指标。分为1-5多个等级，数字越大，触发自动换币的可能性越大。默认值为0，表示当前无风险。5表示该用户正在进行自动换币，4代表该用户即将被进行自动换币，1/2/3表示存在自动换币风险。<br> 适用于`现货模式`/`合约模式`/`跨币种保证金模式`/`组合保证金模式`<br> 当某币种的全平台质押借币量超出平台总上限一定比例时，对于质押该币种且借币量较大的用户，平台将通过自动换币降低质押借币风险。请减少该币种的质押数量或偿还负债，以降低风险。<br> 更多详情，请参阅[平台总质押借币上限说明](https://www.okx.com/zh-hans/help/introduction-to-the-platforms-collateralized-borrowing-limit-mechanism)。 |
| > collateralRestrict | Boolean | ~~平台维度的质押借币限制<br> `true`<br> `false`~~（已弃用，请使用colRes） |
| > collateralEnabled   | Boolean   | `true`：质押币<br>`false`：非质押币<br>适用于`跨币种保证金模式 |
| > colBorrAutoConversion | String | 表示当某个币种的抵押借贷达到平台限制且用户交易账户持有该币种时，强制还款的指标<br> 分为5档，从1到5，数字越小代表强制还款强度越弱。默认为0，表示当前无强制还款风险；5代表用户当前正经历强制还款。<br> 适用于`现货模式`/`合约模式`/`跨币种保证金模式`/`组合保证金模式`|
| > autoLendStatus | String | 自动借出状态<br> `unsupported`：该币种不支持自动借出<br> `off`：自动借出功能关闭<br>`pending`：自动借出功能开启但未匹配<br>`active`：自动借出功能开启且已匹配 |
| > autoLendMtAmt | String | 自动借出已匹配量<br> 当 autoLendStatus 为 `unsupported/off/pending` 时返回 0<br> 当 autoLendStatus 为 `active` 时返回已匹配量 |

<aside class="notice">
当前账户等级下无效字段返回""
</aside>
