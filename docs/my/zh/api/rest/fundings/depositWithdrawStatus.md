---
site: my
lang: zh
name: 获取充值/提现的详细状态
method: GET
path: /api/v5/asset/deposit-withdraw-status
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取充值/提现的详细状态[funding-account-rest-api-get-deposit-withdraw-status]

获取充值与提现的详细状态信息与预估完成时间。

#### 限速：1次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/asset/deposit-withdraw-status`

> 请求示例

```shell
# 查询充值
GET /api/v5/asset/deposit-withdraw-status?txId=xxxxxx&to=1672734730284&ccy=USDT&chain=USDT-ERC20

# 查询提现
GET /api/v5/asset/deposit-withdraw-status?wdId=200045249
```

#### 请求参数
| 参数 | 类型   | 是否必须 | 描述                                              |
| :--- | :----- | :------- |:------------------------------------------------|
| wdId | String | 可选     | 提币申请ID，用于查询资金提现<br>`wdId`与`txId`必传其一也仅可传其一     |
| txId | String | 可选     | 区块转账哈希记录ID，用于查询资金充值<br>`wdId`与`txId`必传其一也仅可传其一 |
| ccy  | String | 可选     | 币种，如`USDT`<br>查询充值时必填，需要与`txId`一并提供               |
| to   | String | 可选     | 资金充值到账账户地址 <br>查询充值时必填，需要与`txId`一并提供              |
| chain | String | 可选    | 币种链信息，如 USDT-ERC20 <br>查询充值时必填，需要与`txId`一并提供     |

> 返回结果

```json
{
    "code":"0",
    "data":[
        {
            "wdId": "200045249",
            "txId": "16f3638329xxxxxx42d988f97", 
            "state": "Pending withdrawal: Wallet is under maintenance, please wait.",
            "estCompleteTime": "01/09/2023, 8:10:48 PM"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名    | 类型   | 描述                                                                                  |
| :-------- | :----- |:------------------------------------------------------------------------------------|
| estCompleteTime | String | 预估完成时间<br>时区为 UTC+8；格式为 MM/dd/yyyy, h:mm:ss AM/PM <br>estCompleteTime仅为预估完成时间，仅供参考 |
| state       | String | 充值/提现的现处于的详细阶段提示<br>冒号前面代表阶段，后面代表状态                                                 |
| txId       | String | 区块转账哈希记录<br>提币如果`txId`已经生成，则返回，否则返回""                                                 |
| wdId       | String | 提币申请ID<br>如查询的是充值，该字段返回""                                                           |

<aside class="notice">
阶段参考<br>
充值<br>
阶段一：监测链上交易<br>
阶段二：推送充值数据到入账环节<br>
阶段三：进行入账<br>
终态：充值已完成<br>
提现<br>
阶段一：等待提现<br>
阶段二：提现中<br>
终态：提现已完成 / 撤销已完成<br>
</aside>
