---
site: okex
lang: zh
name: 预检查账户模式切换
method: GET
path: /api/v5/account/set-account-switch-precheck
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 预检查账户模式切换 [trading-account-rest-api-precheck-account-mode-switch]

获取账户模式切换预检查相关信息

#### 限速：5次/2s

#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/set-account-switch-precheck`

> 请求示例

```shell
GET /api/v5/account/set-account-switch-precheck?acctLv=3

```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| acctLv | String | 是 | 账户模式<br> `1`: 现货模式<br> `2`: 合约模式<br> `3`: 跨币种保证金模式<br> `4`: 组合保证金模式 |

> 返回结果: 合约模式->跨币种，需要现在网页或移动端完成答题

```json
{
    "code": "51070",
    "data": [],
    "msg": "您当前尚未达到升级至该账户模式的要求，请先在官方网站或APP完成账户模式的升级。"
}
```

> 返回结果: 合约模式->跨币种，有不兼容信息。sCode 1

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "3",
            "curAcctLv": "1",
            "mgnAft": null,
            "mgnBf": null,
            "posList": [],
            "posTierCheck": [],
            "riskOffsetType": "",
            "sCode": "1",
            "unmatchedInfoCheck": [
                {
                    "posList": [],
                    "totalAsset": "",
                    "type": "repay_borrowings"
                }
            ]
        }
    ],
    "msg": ""
}
```
> 返回结果: 组合保证金->跨币种，未进行杠杆设置，展示用户全部合约全仓仓位。sCode 3

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "3",
            "curAcctLv": "4",
            "mgnAft": null,
            "mgnBf": null,
            "posList": [
                {
                    "lever": "50",
                    "posId": "2005456500916518912"
                },
                {
                    "lever": "10",
                    "posId": "2005456108363218944"
                },
                {
                    "lever": "100",
                    "posId": "2005456332909477888"
                },
                {
                    "lever": "1",
                    "posId": "2005456415990251520"
                }
            ],
            "posTierCheck": [],
            "riskOffsetType": "",
            "sCode": "3",
            "unmatchedInfoCheck": []
        }
    ],
    "msg": ""
}
```
> 返回结果: 组合保证金->跨币种，已进行杠杆设置，将全部杠杆倍数设置为50，通过梯度档位及保证金校验。sCode 0

```json
{
    "code": "0",
    "data": [
        {
            "acctLv": "3",
            "curAcctLv": "4",
            "mgnAft": {
                "acctAvailEq": "106002.2061970689",
                "details": [],
                "mgnRatio": "148.1652396878421"
            },
            "mgnBf": {
                "acctAvailEq": "77308.89735228613",
                "details": [],
                "mgnRatio": "4.460069474634038"
            },
            "posList": [
                {
                    "lever": "50",
                    "posId": "2005456500916518912"
                },
                {
                    "lever": "50",
                    "posId": "2005456108363218944"
                },
                {
                    "lever": "50",
                    "posId": "2005456332909477888"
                },
                {
                    "lever": "50",
                    "posId": "2005456415990251520"
                }
            ],
            "posTierCheck": [],
            "riskOffsetType": "",
            "sCode": "0",
            "unmatchedInfoCheck": []
        }
    ],
    "msg": ""
}

```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --- | --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sCode | String | 校验码<br> `0`：通过所有验证<br> `1`：有不兼容信息<br> `3`：未进行杠杆设置<br> `4`：梯度档位或保证金校验未通过                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| curAcctLv | String | 当前账户模式<br> `1`: 现货模式<br> `2`: 合约模式<br> `3`: 跨币种保证金模式<br> `4`: 组合保证金模式<br> 所有情况下均返回                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| acctLv | String | 新账户模式<br> `1`: 现货模式<br> `2`: 合约模式<br> `3`: 跨币种保证金模式<br> `4`: 组合保证金模式<br> 所有情况下均返回                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| riskOffsetType | String | ~~风险对冲模式<br> `1`：现货对冲(USDT)<br> `2`：现货对冲(币)<br> `3`：衍生品对冲<br> `4`：现货对冲(USDC)<br> acctLv为`4`时返回，其余情况下返回""<br> 若用户有设置，则为用户的设置值；若没有设置，则为默认值~~（已弃用）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| unmatchedInfoCheck | Array of objects | 包含不匹配信息对象的列表<br> 仅在sCode为`1`，有不兼容信息时返回，其他情况返回[]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| >> type | String | 不匹配信息类型<br> `asset_validation`：资产校验<br> `pending_orders`：撮合挂单<br> `pending_algos`：策略挂单，冰山、时间加权、定投等<br> `isolated_margin`：杠杆逐仓一键借币及自主划转<br> `isolated_contract`：合约逐仓自主划转<br> `contract_long_short`：合约开平模式<br> `cross_margin`：杠杆全仓开仓划转<br> `cross_option_buyer`：期权全仓买方<br> `isolated_option`：期权逐仓 （仅适用于简单账户）<br> `growth_fund`：体验金仓位<br> `all_positions`：所有仓位<br> `spot_lead_copy_only_simple_single`：带单和自定义跟单员只能使用现货或合约模式<br> `stop_spot_custom`：停止现货自定义跟单<br> `stop_futures_custom`：停止合约自定义跟单<br> `lead_portfolio`：身为带单员，您不能切换到组合保证金账户模式<br> `futures_smart_sync`：您存在合约智能跟单，无法切换到现货模式<br>`repay_borrowings`：存在借币<br> `compliance_restriction`：合规，无法使用保证金交易相关服务<br> `compliance_kyc2`：合规，无法使用保证金交易相关服务，如果您不是该地区居民，请进行KYC2身份认证 |
| >> totalAsset | String | 总资产<br> 仅在type为`asset_validation`时返回，其他情况都为""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| >> posList | Array of strings | 不匹配仓位列表，返回持仓ID<br> 在type为仓位相关枚举值时返回，其他情况都为[]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| posList | Array of objects | 合约全仓仓位列表<br> 适用于curAcctLv为`4`，acctLv为`2/3`，且用户具有全仓合约仓位的情况<br> 在sCode为`0/3/4`的情况下返回                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| > posId | String | 持仓ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > lever | String | 切换后的全仓仓位杠杆倍数                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| posTierCheck | Array of objects | 未满足梯度档位校验全仓仓位的列表<br> 仅在sCode为`4`时返回                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > instFamily | String | 交易品种                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > instType | String | 产品类型<br> `SWAP`：永续合约<br> `FUTURES`：交割合约<br> `OPTION`：期权                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > pos | String | 持仓量                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| > lever | String | 杠杆倍数                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| > maxSz | String | 若acctLv为`2/3`，目标账户模式为合约、跨币种，则为当前杠杆倍数下的最大持仓张数；若acctLv为`4`，目标账户模式为组合保证金，则为PM全仓最大持仓量上限                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| mgnBf | Object | 切换账户模式前的保证金相关信息<br> 在sCode为`0/4`时返回，其他时候为null                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > acctAvailEq | String | 美金层面可用保证金<br> 在curAcctLv为`3/4`时返回，其他情况返回""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| > mgnRatio | String | 美金层面维持保证金率<br> 在curAcctLv为`3/4`时返回，其他情况返回""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| > details | Array of objects | 各币种资产详细信息<br> 仅在curAcctLv为`2`时返回，其他情况返回[]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> ccy | String | 币种                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| >> availEq | String | 币种维度可用保证金                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> mgnRatio | String | 币种维度全仓维持保证金率                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| mgnAft | Object | 切换账户模式后的保证金相关信息<br> 在sCode为`0/4`时返回，其他时候为null                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| > acctAvailEq | String | 美金层面可用保证金<br> 在acctLv为`3/4`时返回，其他情况返回""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > mgnRatio | String | 美金层面维持保证金率<br> 在acctLv为`3/4`时返回，其他情况返回""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| > details | Array of objects | 各币种资产详细信息<br> 仅在acctLv为`2`时返回，其他情况返回""                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| >> ccy | String | 币种                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| >> availEq | String | 币种维度可用保证金                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| >> mgnRatio | String | 币种维度全仓维持保证金率                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
