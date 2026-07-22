---
site: us
lang: zh
name: 获取资金划转状态
method: GET
path: /api/v5/asset/transfer-state
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 获取资金划转状态 [funding-account-rest-api-get-funds-transfer-state]

获取最近2个星期内的资金划转状态数据

#### 限速：10 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/transfer-state`

> 请求示例

```shell
GET /api/v5/asset/transfer-state?transId=1&type=1
```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 获取资金划转状态
result = fundingAPI.transfer_state(
    transId="248424899",
    type="0"
)
print(result)

```

#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                                                                                                  |
| :------- | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| transId  | String | 可选       | 划转ID<br>transId和clientId必须传一个，若传两个，以transId为主                                                                                                                                                       |
| clientId  | String | 可选       | 客户自定义ID                                                                                                                                                                                             |
| type     | String | 否       | 划转类型<br>`0`：账户内划转<br>`1`：母账户转子账户(仅适用于母账户APIKey)<br>`2`：子账户转母账户(仅适用于母账户APIKey)<br>`3`：子账户转母账户(仅适用于子账户APIKey)<br>`4`：子账户转子账户(仅适用于子账户APIKey，且目标账户需要是同一母账户下的其他子账户)<br>默认是`0`<br>对于Custody账户该参数可以不传或者传0。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "amt": "1.5",
            "ccy": "USDT",
            "clientId": "",
            "from": "18",
            "instId": "", //已废弃
            "state": "success",
            "subAcct": "test",
            "to": "6",
            "toInstId": "", //已废弃
            "transId": "1",
            "type": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                                                                                                                                             |
| :------ | :----- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| transId | String | 划转 ID                                                                                                                                                          |
| clientId  | String | 客户自定义 ID                                                                                                                                                       |
| ccy     | String | 划转币种                                                                                                                                                           |
| amt     | String | 划转量                                                                                                                                                            |
| type    | String | 划转类型<br>`0`：账户内划转<br>`1`：母账户转子账户(仅适用于母账户APIKey)<br>`2`：子账户转母账户(仅适用于母账户APIKey)<br>`3`：子账户转母账户(仅适用于子账户APIKey)<br>`4`：子账户转子账户(仅适用于子账户APIKey，且目标账户需要是同一母账户下的其他子账户) |
| from    | String | 转出账户<br>`6`：资金账户<br>`18`：交易账户                                                                                                                                  |
| to      | String | 转入账户<br>`6`：资金账户<br>`18`：交易账户                                                                                                                                  |
| subAcct  | String | 子账户名称                                                                                                                                                          |
| instId   | String | 已废弃                                                                                                                                                            |
| toInstId | String | 已废弃                                                                                                                                                            |
| state | String | 转账状态<br>`success`：成功<br>`pending`：处理中<br>`failed`：失败                                                                                                           |
