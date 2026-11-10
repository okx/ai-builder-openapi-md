---
site: us
lang: zh
name: 查看账户持仓风险
method: GET
path: /api/v5/account/account-position-risk
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 查看账户持仓风险 [trading-account-rest-api-get-account-and-position-risk]

查看账户整体风险。

<aside class="notice">
获取同一时间切片上的账户和持仓的基础信息
</aside>

#### 限速：10次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/account-position-risk`

> 请求示例

```shell
GET /api/v5/account/account-position-risk

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户特定风险状态
result = accountAPI.get_account_position_risk()
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instType | String | 否       | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权 <br>|

> 返回结果

```json
{
    "code":"0",
    "data":[
        {
            "adjEq":"174238.6793649711331679",
            "balData":[
                {
                    "ccy":"BTC",
                    "disEq":"78846.7803721021362242",
                    "eq":"1.3863533369419636"
                },
                {
                    "ccy":"USDT",
                    "disEq":"73417.2495112863300127",
                    "eq":"73323.395564963177146"
                }
            ],
            "posData":[
                {
                    "baseBal": "0.4",
                    "ccy": "",
                    "instId": "BTC-USDT",
                    "instType": "MARGIN",
                    "mgnMode": "isolated",
                    "notionalCcy": "0",
                    "notionalUsd": "0",
                    "pos": "0",
                    "posCcy": "",
                    "posId": "310388685292318723",
                    "posSide": "net",
                    "quoteBal": "0"
                }
            ],
            "ts":"1620282889345"
        }
    ],
    "msg":""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                     |
| ----------- | -------- | ------------------------------------------------------------ |
| ts          | String   | 获取账户信息数据的时间，Unix时间戳的毫秒数格式，如 `1597026383085`     |
| adjEq       | String   | 美金层面有效保证金<br>适用于`跨币种保证金模式` 和`组合保证金模式`        |
| balData     | Array of objects    | 币种资产信息                                           |
| > ccy       | String   | 币种                                                         |
| > eq        | String   | 币种总权益                                                   |
| > disEq     | String   | 美金层面币种折算权益                                        |
| posData     | Array of objects    | 持仓详细信息                                           |
| > instType  | String   | 产品类型                                                          |
| > mgnMode   | String   | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓                                                     |
| > posId     | String   | 持仓ID                                         |
| > instId    | String   | 产品ID，如 `BTC-USDT-SWAP`                                                          |
| > pos       | String   | 以`张`为单位的持仓数量，逐仓自主划转模式下，转入保证金后会产生pos为`0`的仓位                                                      |
| > baseBal      | String | ~~交易币余额，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                      |
| > quoteBal     | String | ~~计价币余额 ，适用于 `币币杠杆`（逐仓一键借币模式）~~（已弃用）                                                       |
| > posSide   | String   | 持仓方向<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式（`交割`/`永续`/`期权`：`pos`为正代表开多，`pos`为负代表开空。`币币杠杆`：`posCcy`为交易货币时，代表开多；`posCcy`为计价货币时，代表开空。） |
| > posCcy    | String   | 仓位资产币种，仅适用于`币币杠杆`仓位                                      |
| > ccy       | String   | 占用保证金的币种                                                          |
| > notionalCcy     | String   | 以`币`为单位的持仓数量                                                   |
| > notionalUsd     | String   | 以`美金价值`为单位的持仓数量                                        |
