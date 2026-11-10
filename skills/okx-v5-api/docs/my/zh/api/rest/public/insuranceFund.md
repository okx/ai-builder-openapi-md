---
site: my
lang: zh
name: 获取风险保证金余额
method: GET
path: /api/v5/public/insurance-fund
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取风险保证金余额 [public-data-rest-api-get-insurance-fund]

通过该接口获取系统风险保证金余额信息

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/insurance-fund`

> 请求示例

```shell
GET /api/v5/public/insurance-fund?instType=SWAP&uly=BTC-USD
```

```python
import okx.PublicData as PublicData

flag = "0"  # 实盘:0 , 模拟盘：1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# 获取风险保证金余额
result = publicDataAPI.get_insurance_fund(
    instType="SWAP",
    uly="BTC-USD"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                            |
|:---------------| :----- |:-----|:--------------------------------------------------------------------------------------------------------------|
| instType       | String | 是    | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权                                    |
| type           | String   | 否    | 风险保证金类型<br>`liquidation_balance_deposit`：强平注入 <br>`bankruptcy_loss`：穿仓亏损 <br><del>`platform_revenue`：平台收入注入</del>（已弃用，返回空值。将在后续更新中删除） <br><del>`adl`：自动减仓历史数据</del>（已弃用，返回空值。将在后续更新中删除） <br>默认返回全部类型 |
| instFamily     | String | 可选   | 交易品种<br>`交割`/`永续`/`期权`情况下，`instFamily`必传                                           |
| ccy            | String | 可选   | 币种， 仅适用`币币杠杆`，且必填写                                                                                            |
| before         | String | 否    | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`ts`                                                                            |
| after          | String | 否    | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`ts`                                                                            |
| limit          | String | 否    | 分页返回的结果集数量，最大为100，不填默认返回100条                                                                                  |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "adlType": "",
                    "amt": "1343.1308",
                    "balance": "1369179138.7489",
                    "ccy": "ETH",
                    "maxBal": "",
                    "maxBalTs": "",
                    "ts": "1704883083000",
                    "type": "liquidation_balance_deposit"
                }
            ],
            "instFamily": "ETH-USD",
            "instType": "OPTION",
            "total": "1369179138.7489"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型**           | **描述**                                    |
| :--------- |:-----------------|:------------------------------------------|
| total        | String           | 平台风险保证金总计，单位为USD                          |
| instFamily        | String           | 交易品种<br>适用于`交割`/`永续`/`期权`                     |
| instType | String | 产品类型 |
| details        | Array of objects | 风险保证金详情                                   |
| > balance     | String           | 风险保证金总量                                   |
| > amt       | String           | 风险保证金更新数量 <br>在type为`liquidation_balance_deposit`或`bankruptcy_loss`时适用                                 |
| > ccy     | String           | 风险保证金总量对应的币种                              |
| > type     | String           | 风险保证金类型<br>`liquidation_balance_deposit`：强平注入<br>`bankruptcy_loss`：穿仓亏损<br><del>`platform_revenue`：平台收入注入</del>（已弃用，返回空值）<br><del>`adl`：自动减仓历史数据</del>（已弃用，返回空值）                                   |
| > maxBal | String | ~~过去八小时内的风险保证金余额最大值 <br>仅在type为`adl`时适用~~（已弃用，返回空值） |
| > maxBalTs | String | ~~过去八小时内风险保证金余额最大值对应的时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` <br>仅在type为`adl`时适用~~（已弃用，返回空值） |
| > decRate | String | ~~风险保证金实时下降率（balance与maxBal相比较） <br>仅在type为`adl`时适用~~（已弃用） |
| > adlType | String | ~~关于自动减仓的事件 <br>`rate_adl_start`：由于风险保证金下降率过高造成的自动减仓开始 <br>`bal_adl_start`：由于风险保证金余额下降过高造成的自动减仓开始 <br>`pos_adl_start`：由于强平单的规模积累到一定程度的自动减仓开始（仅适用于盘前交易市场）<br>`adl_end`：自动减仓结束 <br>仅在type为`adl`时适用~~（已弃用，返回空值） |
| > ts        | String           | 风险保证金更新时间，Unix时间戳的毫秒数格式，如 `1597026383085` |

<aside class="notice">
`regular_update` 类型已被删除。`adl` 和 `platform_revenue` 类型已弃用，当前返回空值；将在后续更新中删除。`amt` 字段用于展示 type 为 `liquidation_balance_deposit` 或 `bankruptcy_loss` 时的风险保证金余额差值，数据一天产生一次，每天下午4点左右（UTC 8）更新。
</aside>
