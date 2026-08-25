---
site: us
lang: zh
name: 调整保证金
method: POST
path: /api/v5/account/position/margin-balance
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 调整保证金 [trading-account-rest-api-increase-decrease-margin]

增加或者减少逐仓保证金。减少保证金可能会导致实际杠杆倍数发生变化。

#### 限速：20次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/position/margin-balance`

> 请求示例

```shell
POST /api/v5/account/position/margin-balance 
body
{
    "instId":"BTC-USDT-SWAP",
    "posSide":"short",
    "type":"add",
    "amt":"1"
}

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 调整保证金
result = accountAPI.adjustment_margin(
    instId="BTC-USDT-SWAP",
    posSide="short",
    type= "add",
    amt="1"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                     |
| :------ | :----- |:-----|:-----------------------------------------------------------------------|
| instId  | String | 是    | 产品ID                                                                   |
| posSide | String | 是    | 持仓方向，默认值是`net`<br>`long`：开平仓模式开多<br>`short`：开平仓模式开空<br>`net`：买卖模式 |
| type    | String | 是    | 增加/减少保证金<br>`add`：增加 <br>`reduce`：减少     |
| amt     | String | 是    | 增加或减少的保证金数量                                                            |
| ccy     | String | 可选   | 增加或减少的保证金的币种，<br>适用于`逐仓杠杆`仓位                                |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0.3",
            "ccy": "BTC",
            "instId": "BTC-USDT",
            "leverage": "",
            "posSide": "net",
            "type": "add"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                |
| :--------- | :------- | :-------------------------------------- |
| instId     | String   | 产品ID                              |
| posSide    | String   | 持仓方向 |
| amt        | String   | 已增加/减少的保证金数量                 |
| type       | String   | 增加/减少保证金                         |
| leverage   | String   | 调整保证金后的实际杠杆倍数                         |
| ccy        | String   | 增加或减少的保证金的币种                         |

<aside class="notice">
自主划转模式<br>
初始划入逐仓仓位的保证金价值必须大于等于1万USDT,账户上会产生一个仓位。
</aside>
