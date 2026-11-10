---
site: turkey
lang: zh
name: 子账户间资金划转
method: POST
path: /api/v5/asset/subaccount/transfer
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 子账户间资金划转 [sub-account-rest-api-master-accounts-manage-the-transfers-between-sub-accounts]

母账户控制子账户与子账户之间划转（仅适用于母账户）

调用时，APIKey 需要有划转权限

#### 限速：1次/s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/asset/subaccount/transfer`

> 请求示例

```shell
POST /api/v5/asset/subaccount/transfer
body
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "fromSubAccount":"test-1",
    "toSubAccount":"test-2"
}
```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 子账户间资金划转
result = subAccountAPI.subAccount_transfer(
    ccy="USDT",
    amt="10",
    froms="6",
    to="6",
    fromSubAccount="test-1",
    toSubAccount="test-2"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                           |
| :------ | :----- | :------- |:---------------------------------------------|
| ccy  | String | 是      | 币种                                           |
| amt | String | 是       | 划转数量                                         |
| from   | String | 是       | 转出子账户类型<br>`6`：资金账户<br>`18`：交易账户             |
| to  | String | 是       | 转入子账户类型<br>`6`：资金账户<br>`18`：交易账户             |
| fromSubAccount   | String | 是       | 转出子账户的子账户名称                                  |
| toSubAccount   | String | 是       | 转入子账户的子账户名称                                  |
| loanTrans  | Boolean | 否    | 是否支持`跨币种保证金模式`或`组合保证金模式`下的借币转入/转出<br>默认`false` |
| omitPosRisk  | String | 否   | 是否忽略仓位风险<br>默认为`false`<br>仅适用于`组合保证金模式`      |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "transId":"12345",
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                              |
| :--------- | :------- | :---------------------------------------------------- |
| transId     | String   | 划转ID                 |
