---
site: us
lang: zh
name: 查询子账户转账记录
method: GET
path: /api/v5/asset/subaccount/bills
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---


### 查询子账户转账记录 [sub-account-rest-api-get-history-of-sub-account-transfer]

仅适用于母账户。转账记录可追溯至2022年9月28日。

#### 限速：6次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/asset/subaccount/bills`

> 请求示例

```shell
GET /api/v5/asset/subaccount/bills
```

```python
import okx.SubAccount as SubAccount

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

subAccountAPI = SubAccount.SubAccountAPI(apikey, secretkey, passphrase, False, flag)

# 查询子账户转账记录
result = subAccountAPI.bills()
print(result)
```

#### 请求参数

| 参数名      | 类型       | 是否必须 | 描述                                                            |
|:---------|:---------| :------- |:--------------------------------------------------------------|
| ccy      | String   | 否       | 币种，如 BTC                                                      |
| type     | String   | 否       | 划转类型<br>`0`：母账户转子账户<br>`1`：子账户转母账户                            |
| subAcct  | String   | 否       | 子账户名称                                                         |
| after    | String   | 否       | 查询在billId创建时间之前(不包含)的内容，值为时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` |
| before   | String   | 否       | 查询在billId创建时间之后(不包含)的内容，值为时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` |
| limit    | String   | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条                                  |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [
      {
        "amt": "1.1",
        "billId": "89887685",
        "ccy": "USDT",
        "subAcct": "hahatest1",
        "ts": "1712560959000",
        "type": "0"
      }
    ]
}
```

#### 返回参数

| **参数名**   | **类型**     | **描述**                                   |
|:----------|:-----------|:-----------------------------------------|
| billId    | String     | 账单ID                                     |
| ccy       | String     | 划转币种                                     |
| amt       | String     | 划转金额                                     |
| type      | String     | 账单类型                                     |
| subAcct   | String     | 子账户名称                                    |
| ts        | String     | 账单ID创建时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
