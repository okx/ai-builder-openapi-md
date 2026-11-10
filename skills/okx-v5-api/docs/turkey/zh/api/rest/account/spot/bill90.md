---
site: turkey
lang: zh
name: 账单流水查询（近三月）
method: GET
path: /api/v5/account/bills-archive
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 账单流水查询（近三月）[trading-account-rest-api-get-bills-details-last-3-months]

帐户资产流水是指导致帐户余额增加或减少的行为。本接口可以查询最近3个月的账单数据。

#### 限速：5次/2s 
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/bills-archive`

> 请求示例

```shell
GET /api/v5/account/bills-archive

GET /api/v5/account/bills-archive?instType=SPOT

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看账户账单详情 （近三个月内）
result = accountAPI.get_account_bills_archive()
print(result)
```

#### 请求参数

| 参数名       | 类型   | 是否必须   | 描述                                                                |
|:----------| :----- |:-------|:------------------------------------------------------------------|
| instType  | String | 否      | 产品类型<br>`SPOT`：币币                                                 |
| ccy       | String | 否      | 账单币种                                                              |
| type      | String | 否      | 账单类型<br>`1`：划转<br>`2`：交易<br>`6`：保证金划转                                          |
| subType   | String | 否      | 账单子类型<br>`1`：买入<br>`2`：卖出<br>`11`：转入<br>`12`：转出<br>`236`：兑换主流币用户账户转入<br>`237`：兑换主流币用户账户转出 |
| after     | String | 否      | 请求此id之前（更旧的数据）的分页内容，传的值为对应接口的`billId`                             |
| before    | String | 否      | 请求此id之后（更新的数据）的分页内容，传的值为对应接口的`billId`                             |
| begin     | String | 否      | 筛选的开始时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085`                           |
| end       | String | 否      | 筛选的结束时间戳，Unix 时间戳为毫秒数格式，如 `1597027383085`                           |
| limit     | String | 否      | 分页返回的结果集数量，最大为100，不填默认返回100条                                      |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "bal": "8694.2179403378290202",
        "balChg": "0.0219338232210000",
        "billId": "623950854533513219",
        "ccy": "USDT",
        "clOrdId": "",
        "earnAmt": "",
        "earnApr": "",        
        "execType": "T",
        "fee": "-0.000021955779",
        "fillFwdPx": "",
        "fillIdxPx": "27104.1",
        "fillMarkPx": "",
        "fillMarkVol": "",
        "fillPxUsd": "",
        "fillPxVol": "",
        "fillTime": "1695033476166",
        "from": "",
        "instId": "BTC-USDT",
        "instType": "SPOT",
        "interest": "0",
        "mgnMode": "isolated",
        "notes": "",
        "ordId": "623950854525124608",
        "pnl": "0",
        "posBal": "0",
        "posBalChg": "0",
        "px": "27105.9",
        "subType": "1",
        "sz": "0.021955779",
        "tag": "",
        "to": "",
        "tradeId": "586760148",
        "ts": "1695033476167",
        "type": "2"
    }]
} 
```

#### 返回参数

| 参数名         | 类型   | 描述                                                                                    |
|:------------| :----- |:--------------------------------------------------------------------------------------|
| instType    | String | 产品类型                                                                                  |
| billId      | String | 账单ID                                                                                  |
| type        | String | 账单类型                                                                                  |
| subType     | String | 账单子类型                                                                                 |
| ts          | String | 余额更新完成的时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                             |
| balChg      | String | 账户层面的余额变动数量                                                                           |
| posBalChg   | String | 仓位层面的余额变动数量                                                                           |
| bal         | String | 账户层面的余额数量                                                                             |
| posBal      | String | 仓位层面的余额数量                                                                             |
| sz          | String | 数量<br/>对于交割、永续以及期权，为成交或者持仓的数量，单位为张，总为正数。<br/>其他情况下，单位为账户余额币种（`ccy`）。                                                                                                                                                                                                                                                                                                                                                                                                                   |
| px          | String | 价格，与 subType 相关<br><li>为成交价格时有</li>`1`：买入 `2`：卖出                                      |
| ccy         | String | 账户余额币种                                                                                |
| pnl         | String | 收益                                                                                    |
| fee         | String | 手续费<br>正数代表平台返佣 ，负数代表平台扣除                                                             |
| earnAmt | String | 自动赚币数量<br> 仅适用于type 381 |
| earnApr | String | 自动赚币实际年利率 <br> 仅适用于type 381 |
| mgnMode     | String | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓<br>`cash`：非保证金<br/>如果账单不是由交易产生的，该字段返回 "" |
| instId      | String | 产品ID，如 `BTC-USDT`                                                                     |
| ordId       | String | 订单ID<br>当type为`2`：交易时，返回相应订单id。无订单时，该字段返回 ""                                          |
| execType    | String | 流动性方向<br>`T`：taker<br>`M`：maker                                                       |
| from        | String | 转出账户<br>`6`：资金账户<br>`18`：交易账户<br>仅适用于`资金划转`，不是`资金划转`时，返回 ""                           |
| to          | String | 转入账户<br>`6`：资金账户<br>`18`：交易账户<br>仅适用于`资金划转`，不是`资金划转`时，返回 ""                               |
| notes       | String | 备注                                                                                    |
| interest    | String | 利息                                                                                    |
| tag         | String | 订单标签 <br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                      |
| fillTime    | String | 最新成交时间                                                                                |
| tradeId     | String | 最新成交ID                                                                                |
| clOrdId     | String | 客户自定义订单ID                                                                             |
| fillIdxPx   | String | 交易执行时的指数价格 <br>对于交叉现货币对，返回 baseCcy-USDT 的指数价格。 如LTC-ETH，该字段返回LTC-USDT的指数价格。          |
| fillMarkPx  | String | 成交时的标记价格，仅适用于 `交割`/`永续`/`期权`                                                                |
| fillPxVol   | String | 成交时的隐含波动率，仅适用于`期权`，其他业务线返回空字符串""                                                        |
| fillPxUsd   | String | 成交时的期权价格，以USD为单位，仅适用于`期权`，其他业务线返回空字符串""                                                 |
| fillMarkVol | String | 成交时的标记波动率，仅适用于`期权`，其他业务线返回空字符串""                                                        |
| fillFwdPx   | String | 成交时的远期价格，仅适用于`期权`，其他业务线返回空字符串""                                                         |
