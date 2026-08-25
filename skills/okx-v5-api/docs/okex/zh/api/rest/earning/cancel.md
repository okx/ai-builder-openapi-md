---
site: okex
lang: zh
name: POST / 撤销项目申购/赎回
method: POST
path: /api/v5/finance/staking-defi/cancel
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤销项目申购/赎回 [financial-product-earn-post-cancel-purchases-redemptions]

<aside class="notice">
撤销申购后的资金返回资金账户。
</aside>

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/staking-defi/cancel`

> 请求示例

```shell
POST /api/v5/finance/staking-defi/cancel
body 
{
    "ordId":"754147",
    "protocolType":"defi"
}
```

```python
import okx.Finance.StakingDefi as StakingDefi

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘:0 , 模拟盘:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.cancel(
           ordId = "1234",
           protocolType = "defi"
        )
print(result)
```

#### 请求参数

| 参数  | 类型    | 是否必须  | 描述                    |
| :--- | :-----  | :------- | :---------------------- |
| ordId          | String | 是   | 订单ID               |
| protocolType   | String | 是   | 项目类型<br>`defi`：链上赚币 |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "ordId": "754147",
      "tag": ""
    }
  ]
}
```

#### 返回参数

| 参数名   | 类型     | 描述   |
|-------|--------|------|
| ordId | String | 订单ID |
| tag   | String | 订单标签 |
