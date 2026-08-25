---
site: my
lang: zh
name: 获取计息记录
method: GET
path: /api/v5/account/interest-accrued
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取计息记录 [trading-account-rest-api-get-interest-accrued-data]

获取过去一年的计息记录

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/interest-accrued`

> 请求示例

```shell
GET /api/v5/account/interest-accrued

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取计息记录
result = accountAPI.get_interest_accrued()
print(result)
```

#### 请求参数

| 参数名     | 类型   | 是否必须    | 描述                                                   |
|:--------| :----- |:--------|:-----------------------------------------------------|
| type    | String | 否       | 借币类型<br>`2`：市场借币<br>默认为`2`                           |
| ccy     | String | 否       | 借贷币种，如 `BTC`<br>仅适用于`市场借币`<br>仅适用于`币币杠杆`             |
| instId  | String | 否       | 产品ID，如 `BTC-USDT`<br>仅适用于`市场借币`                      |
| mgnMode | String | 否       | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓<br>仅适用于`市场借币`   |
| after   | String | 否       | 请求此时间戳之前（更旧的数据）的分页内容，Unix时间戳的毫秒数格式，如 `1597026383085` |
| before  | String | 否       | 请求此时间戳之后（更新的数据）的分页内容，Unix时间戳的毫秒数格式，如 `1597026383085` |
| limit   | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条                         |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "instId": "",
            "interest": "0.0003960833333334",
            "interestRate": "0.0000040833333333",
            "liab": "97",
            "totalLiab": "",
            "interestFreeLiab": "",
            "mgnMode": "",
            "ts": "1637312400000",
            "type": "1"
        },
        {
            "ccy": "USDT",
            "instId": "",
            "interest": "0.0004083333333334",
            "interestRate": "0.0000040833333333",
            "liab": "100",
            "totalLiab": "",
            "interestFreeLiab": "",
            "mgnMode": "",
            "ts": "1637049600000",
            "type": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**      | **类型**   | **描述**                               |
|--------------|----------|--------------------------------------|
| type         | String   | 类型<br>`2`：市场借币                       |
| ccy          | String   | 借贷币种，如 `BTC`                         |
| instId       | String   | 产品ID，如 `BTC-USDT`<br>仅适用于`市场借币`      |
| mgnMode      | String   | 保证金模式<br>`cross`：全仓<br>`isolated`：逐仓 |
| interest     | String   | 利息累计                                 |
| interestRate | String   | 借款计息利率(小时)                           |
| liab         | String   | 计息负债                                 |
| totalLiab               | String          | 当前账户总负债量                                 |
| interestFreeLiab               | String          | 当前账户免息负债量                                 |
| ts           | String   | 计息时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
