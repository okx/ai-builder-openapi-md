---
site: us
lang: zh
name: 查看历史持仓信息
method: GET
path: /api/v5/account/positions-history
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 查看历史持仓信息 [trading-account-rest-api-get-positions-history]

获取最近3个月有更新的仓位信息，按照仓位更新时间倒序排列。于**2024年11月11日中午12:00（UTC+8）**开始支持组合保证金账户模式下的历史持仓。

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/positions-history`

> 请求示例

```shell
GET /api/v5/account/positions-history
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看历史持仓信息
result = accountAPI.get_positions_history()
print(result)
```

#### 请求参数 

| 参数名      | 类型       | 是否必须 | 描述                                                                                   |
|:---------|:---------|:-----|:-------------------------------------------------------------------------------------|
| instType | String   | 否    | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| instId   | String   | 否    | 交易产品ID，如：`BTC-USD-SWAP`                                                              |
| mgnMode  | String   | 否    | 保证金模式<br>`cross`：全仓，`isolated`：逐仓                                                    |
| type     | String   | 否    | 最近一次平仓的类型<br>`1`：部分平仓;`2`：完全平仓;`3`：强平;`4`：强减; `5`：ADL自动减仓 - 仓位未完全平仓; `6`：ADL自动减仓 - 仓位完全平仓<br>状态叠加时，以最新的平仓类型为准状态为准。 |
| posId    | String   | 否    | 持仓ID。存在有效期的属性，自最近一次完全平仓算起，满30天 posId 会失效，之后的仓位，会使用新的 posId。                          |
| after    | String   | 否    | 查询仓位更新 (uTime) 之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                          |
| before   | String   | 否    | 查询仓位更新 (uTime) 之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                          |
| limit    | String   | 否    | 分页返回结果的数量，最大为100，默认100条，uTime 相同的记录均会在当前请求中全部返回                                      |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "cTime": "1654177169995",
            "ccy": "BTC",
            "closeAvgPx": "29786.5999999789081085",
            "closeTotalPos": "1",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "lever": "10.0",
            "mgnMode": "cross",
            "openAvgPx": "29783.8999999995535393",
            "openMaxPos": "1",
            "realizedPnl": "0.001",
            "fee": "-0.0001",
            "fundingFee": "0",
            "liqPenalty": "0",
            "pnl": "0.0011",
            "pnlRatio": "0.000906447858888",
            "posId": "452587086133239818",
            "posSide": "long",
            "direction": "long",
            "triggerPx": "",
            "type": "1",
            "uTime": "1654177174419",
            "uly": "BTC-USD",
            "nonSettleAvgPx":"",
            "settledPnl":""
        }
    ],
    "msg": ""
}
```

#### 返回参数 

| 参数名                | 类型       | 描述                                                                                            |
|:-------------------|:---------|:----------------------------------------------------------------------------------------------|
| instType           | String   | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| instId             | String   | 交易产品ID                                                                                        |
| mgnMode            | String   | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓                                                          |
| type               | String   | 最近一次平仓的类型<br>`1`：部分平仓<br>`2`：完全平仓<br>`3`：强平<br>`4`：强减<br>`5`：ADL自动减仓<br>状态叠加时，以最新的平仓类型为准状态为准。 |
| cTime              | String   | 仓位创建时间                                                                                        |
| uTime              | String   | 仓位更新时间                                                                                        |
| openAvgPx          | String   | 开仓均价<br>会随结算周期变化，特别是在交割合约全仓模式下，结算时开仓均价会更新为结算价格，同时新增头寸也会改变开仓均价。                                |
| nonSettleAvgPx     | String   | 未结算均价<br>不受结算影响的加权开仓价格，仅在新增头寸时更新，和开仓均价的主要区别在于是否受到结算影响。<br>仅适用于`全仓``交割`                        |
| closeAvgPx         | String   | 平仓均价                                                                                          |
| posId              | String   | 仓位ID                                                                                          |
| openMaxPos         | String   | 最大持仓量                                                                                         |
| closeTotalPos      | String   | 累计平仓量                                                                                         |
| realizedPnl        | String   | 已实现收益<br>仅适用于`交割`/`永续`/`期权`<br>`realizedPnl`=`pnl`+`fee`+`fundingFee`+`liqPenalty`+`settledPnl`   |
| settledPnl         | String   | 已实现收益<br>仅适用于`全仓``交割`                                                                         |
| pnlRatio           | String   | 已实现收益率                                                                                        |
| fee                | String   | 累计手续费金额<br>正数代表平台返佣，负数代表平台扣除。                                                                 |
| fundingFee         | String   | 累计资金费用                                                                                        |
| liqPenalty         | String   | 累计爆仓罚金，有值时为负数。                                                                                |
| pnl                | String   | 已实现收益(不包括手续费)                                                                                        |
| posSide            | String   | 持仓模式方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式                                     |
| lever              | String   | 杠杆倍数                                                                                          |
| direction          | String   | 持仓方向<br>`long`：多<br>`short`：空<br>仅适用于 `杠杆`/`交割`/`永续`/`期权`                                     |
| triggerPx          | String   | 触发标记价格<br>`type` 为`3`,`4`,`5`时有值；为`1`, `2` 时为空                                                |
| uly                | String   | 标的指数                                                                                          |
| ccy                | String   | 占用保证金的币种                                                                                      |
