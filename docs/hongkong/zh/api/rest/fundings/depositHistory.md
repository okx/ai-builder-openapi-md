---
site: hongkong
lang: zh
name: 获取充值记录
method: GET
path: /api/v5/asset/deposit-history
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取充值记录 [funding-account-rest-api-get-deposit-history]

根据币种，充值状态，时间范围获取充值记录，按照时间倒序排列，默认返回 100 条数据。<br>
支持Websocket订阅，参考 [充值信息频道](/docs-v5/zh/#funding-account-websocket-deposit-info-channel)。

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/deposit-history`

> 请求示例

```shell
# 查询最近的充值记录
GET /api/v5/asset/deposit-history

# 查询从2022年06月01日到2022年07月01日之间的BTC的充值记录
GET /api/v5/asset/deposit-history?ccy=BTC&after=1654041600000&before=1656633600000

```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 获取充值记录
result = fundingAPI.get_deposit_history()
print(result)
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                                                                                       |
|:-------| :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------|
| ccy    | String | 否       | 币种名称，如 `BTC`                                                                                                                             |
| depId  | String | 否     | 充值记录 ID                                                                                                                                  |
| fromWdId  | String | 否     | 内部转账发起者提币申请 ID<br>如果该笔充值来自于内部转账，则该字段展示内部转账发起者的提币申请 ID                                                                                    |
| txId   | String | 否      | 区块转账哈希记录                                                                                                                                 |
| type   | String | 否      | 充值方式<br>`3`：内部转账<br>`4`：链上充值                                                                                                             |
| state  | String | 否       | 充值状态<br>`0`：等待确认<br>`1`：确认到账<br>`2`：充值成功<br>`8`：因该币种暂停充值而未到账，恢复充值后自动到账<br>`11`：命中地址黑名单<br>`12`：账户或充值被冻结<br>`13`：子账户充值拦截<br>`14`：KYC限额<br> `17`：钱包地址正等待国际转账规则认证    |
| after  | String | 否       | 查询在此之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1654041600000`                                                                                         |
| before | String | 否       | 查询在此之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1656633600000`                                                                                         |
| limit  | string | 否       | 返回的结果集数量，默认为100，最大为100，不填默认返回100条                                                                                                        |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "actualDepBlkConfirm": "2",
        "amt": "1",
        "areaCodeFrom": "",
        "ccy": "USDT",
        "chain": "USDT-TRC20",
        "depId": "88****33",
        "from": "",
        "fromWdId": "",
        "state": "2",
        "to": "TN4hGjVXMzy*********9b4N1aGizqs",
        "ts": "1674038705000",
        "txId": "fee235b3e812********857d36bb0426917f0df1802"
    }
  ]
}
```

#### 返回参数

| 参数名                 | 类型     | 描述                                                                                                                                       |
|---------------------|--------|------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                 | String | 币种名称，如 `BTC`                                                                                                                             |
| chain               | String | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链                                                                          |
| amt                 | String | 充值数量                                                                                                                                     |
| from                | String | 充值账户<br>如果该笔充值来自于内部转账，则该字段展示内部转账发起者的账户信息，可以是手机号或邮箱（脱敏），其他情况返回""                                                                         |
| areaCodeFrom        | String | 如果`from`为手机号，该字段为该手机号的区号                                                                                                                 |
| to                  | String | 到账地址<br>如果该笔充值来自于链上充值，则该字段展示链上地址，其他情况返回""                                                                                                |
| txId                | String | 区块转账哈希记录                                                                                                                                 |
| ts                  | String | 充值记录创建时间，Unix 时间戳的毫秒数格式，如 `1655251200000`                                                                                                |
| state               | String | 充值状态<br>`0`：等待确认 <br>`1`：确认到账 <br>`2`：充值成功 <br>`8`：因该币种暂停充值而未到账，恢复充值后自动到账<br>`11`：命中地址黑名单<br>`12`：账户或充值被冻结<br>`13`：子账户充值拦截<br>`14`：KYC限额 |
| depId               | String | 充值记录 ID                                                                                                                                  |
| fromWdId            | String | 内部转账发起者提币申请 ID<br>如果该笔充值来自于内部转账，则该字段展示内部转账发起者的提币申请 ID，其他情况返回""                                                                           |
| actualDepBlkConfirm | String | 最新的充币网络确认数                                                                                                                               |

<aside class="notice">
关于充值状态<br>
<b>等待确认</b>是没有达到充币确认数。<br>
<b>确认到账</b>是够充币确认数，且币已经到账户中，但是不可提。<br>
<b>充值成功</b>是当前账户完成到提币确认，可以提出。
</aside>
