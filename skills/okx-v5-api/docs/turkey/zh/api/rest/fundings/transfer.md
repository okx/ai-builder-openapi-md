---
site: turkey
lang: zh
name: 资金划转
method: POST
path: /api/v5/asset/transfer
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 资金划转 [funding-account-rest-api-funds-transfer]

调用时，API Key 需要有划转权限。

支持母账户的资金账户划转到交易账户，母账户到子账户的资金账户和交易账户划转。

子账户默认可转出至母账户，划转到同一母账户下的其他子账户，需要先调用 [设置子账户主动转出权限](/docs-v5/zh/#sub-account-rest-api-set-permission-of-transfer-out) 接口进行授权。

<aside class="notice">
请求的成功或失败不一定反映实际的划转结果，建议通过调用"获取资金划转状态"接口来确认最终结果。
</aside>

#### 限速：2 次/s
#### 限速规则：User ID + Currency

#### HTTP 请求

`POST /api/v5/asset/transfer`

> 请求示例

```shell
# 母账户USDT从资金账户划转1.5USDT到交易账户
POST /api/v5/asset/transfer
body 
{
    "ccy":"USDT",
    "amt":"1.5",
    "from":"6",
    "to":"18"
}

# 母账户从资金账户划转1.5USDT到子账户的资金账户
POST /api/v5/asset/transfer
body 
{
    "ccy":"USDT",
    "type":"1",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}

# 子账户从资金账户划转1.5USDT到另一子账户的资金账户
POST /api/v5/asset/transfer
body 
{
    "ccy":"USDT",
    "type":"4",
    "amt":"1.5",
    "from":"6",
    "to":"6",
    "subAcct":"mini"
}
```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 资金划转
result = fundingAPI.funds_transfer(
    ccy="USDT",
    amt="1.5",
    from_="6",
    to="18"
)
print(result)
```

#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                                                                                                                                                                                           |
| :------- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type     | String | 否       | 划转类型<br>`0`：账户内划转<br>`1`：母账户转子账户(仅适用于母账户APIKey)<br>`2`：子账户转母账户(仅适用于母账户APIKey)<br>`3`：子账户转母账户(仅适用于子账户APIKey)<br>`4`：子账户转子账户(仅适用于子账户APIKey，且目标账户需要是同一母账户下的其他子账户。子账户主动转出权限默认是关闭的，权限调整参考 [设置子账户主动转出权限](/docs-v5/zh/#sub-account-rest-api-set-permission-of-transfer-out)。)<br>默认是`0`<br>如果您希望通过母账户API Key控制子账户之间的划转，参考接口 [子账户间资金划转](/docs-v5/zh/#sub-account-rest-api-master-accounts-manage-the-transfers-between-sub-accounts) |
| ccy      | String | 是       | 划转币种，如 `USDT`                                                                                                                                                                                                                                                                                                                                                                                                                |
| amt      | String | 是       | 划转数量                                                                                                                                                                                                                                                                                                                                                                                                                         |
| from     | String | 是       | 转出账户<br>`6`：资金账户<br>`18`：交易账户                                                                                                                                                                                                                                                                                                                                                                                                |
| to       | String | 是       | 转入账户<br>`6`：资金账户<br>`18`：交易账户                                                                                                                                                                                                                                                                                                                                                                                                |
| subAcct  | String | 可选     | 子账户名称<br>当`type`为`1`/`2`/`4`时，该字段必填                                                                                                                                                                                                                                                                                                                                                                                          |
| loanTrans  | Boolean | 否    | 是否支持`现货模式`/`跨币种保证金模式`/`组合保证金模式`下的借币转出<br>`true`：支持借币转出<br>`false`：不支持借币转出<br>默认为`false`                                                                                                                                                                                                                                                                                                                                      |
| omitPosRisk  | String | 否   | 是否忽略仓位风险<br>默认为`false`<br>仅适用于`组合保证金模式`                                                                                                                                                                                                                                                                                                                                                                                      |
| clientId  | String | 否    | 客户自定义ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                                                                                                                                                                                           |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "transId": "754147",
      "ccy": "USDT",
      "clientId": "",
      "from": "6",
      "amt": "0.1",
      "to": "18"
    }
  ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述     |
| :------ | :----- | :------- |
| transId | String | 划转 ID  |
| ccy     | String | 划转币种 |
| from    | String | 转出账户 |
| amt     | String | 划转量   |
| to      | String | 转入账户 |
| clientId  | String | 客户自定义ID     |
