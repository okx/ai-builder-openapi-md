---
site: turkey
lang: zh
name: 查看子账户列表
method: GET
path: /api/v5/users/subaccount/list
module: users
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

## REST API [sub-account-rest-api]
### 查看子账户列表 [sub-account-rest-api-get-sub-account-list]

仅适用于母账户

#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/users/subaccount/list`

> 请求示例

```shell
GET /api/v5/users/subaccount/list

```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 查看子账户列表
result = subAccountAPI.get_subaccount_list()
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                   |
| :------ | :----- | :------- | :----------------------------------------------------- |
| enable  | String | 否       | 子账户状态 <br>`true`: 正常使用   `false`: 冻结   |
| subAcct | String | 否       | 子账户名称   |
| after   | String | 否       | 查询在此之前的内容，值为子账户创建时间戳，Unix时间戳为毫秒数格式 |
| before  | String | 否       | 查询在此之后的内容，值为子账户创建时间戳，Unix时间戳为毫秒数格式 |
| limit   | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条     |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "canTransOut": false,
            "enable": true,
            "frozenFunc": [
            ],
            "gAuth": false,
            "label": "D456DDDLx",
            "mobile": "",
            "subAcct": "D456DDDL",
            "ts": "1659334756000",
            "type": "1",
            "uid": "3400***********7413",
            "subAcctLv": "1",
            "firstLvSubAcct": "D456DDDL",
            "ifDma": false
        }
    ]
}
```

#### 返回参数

| **参数名**     | **类型**          | **描述**                                                                                                                    |
|:------------|:----------------|:--------------------------------------------------------------------------------------------------------------------------|
| type        | String          | 子账户类型 <br>`1`：普通子账户 <br>`2`：资管子账户 <br>`5`：托管交易子账户 - Copper<br>  `9`：资管交易子账户 - Copper<br> `12`：托管交易子账户 - Komainu                                                               |
| enable      | Boolean         | 子账户状态 <br>`true`：正常使用 `false`：冻结（全局）                                                                                      |
| subAcct     | String          | 子账户名称                                                                                                                     |
| uid         | String          | 子账户UID                                                                                                                    |
| label       | String          | 子账户备注                                                                                                                     |
| mobile      | String          | 子账户绑定手机号                                                                                                                  |
| gAuth       | Boolean         | 子账户是否开启的登录时的谷歌验证<br>`true`：已开启<br>`false`：未开启                                                                             |
| frozenFunc  | Array of strings | 被冻结的功能<br>`trading`：交易<br>`convert`：闪兑<br>`transfer`：母子账户间资金划转<br>`withdrawal`：提币<br>`deposit`：充值<br>`flexible_loan`：活期借币 |
| canTransOut | Boolean         | 是否可以主动转出<br>`true`：可以转出 <br>`false`：不可转出                                                                                  |
| ts          | String          | 子账户创建时间，Unix时间戳的毫秒数格式 ，如 `1597026383085`                                                                                  |
| subAcctLv      | String  | 子账户层级 <br>`1`: 一级子账号<br/>`2`: 二级子账户                                                                     |
| firstLvSubAcct | String  | 一级子账号 <br>对于 subAcctLv: 1, firstLvSubAcct 与 subAcct 相等。<br/>对于 subAcctLv: 2, subAcct 属于 firstLvSubAcct。 |
| ifDma          | Boolean | 是否为 DMA 经济商子账号。 <br>`true`: DMA 经济商子账号。<br/> `false`: 非 DMA 经济商子账号。                                     |
