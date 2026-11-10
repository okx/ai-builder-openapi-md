---
site: okex
lang: zh
name: POST / 申购项目
method: POST
path: /api/v5/finance/staking-defi/purchase
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 申购项目 [financial-product-earn-post-purchase]

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/staking-defi/purchase`

> 请求示例

```shell
# 投资100ZIL30天的锁仓挖矿项目
POST /api/v5/finance/staking-defi/purchase
body 
{
    "productId":"1234",
    "investData":[
      {
        "ccy":"ZIL",
        "amt":"100"
      }
    ],
    "term":"30"
}
```

```python
import okx.Finance.StakingDefi as StakingDefi

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0" # 实盘:0 , 模拟盘:1

StakingAPI = StakingDefi.StakingDefiAPI(apikey, secretkey, passphrase, False, flag)

result = StakingAPI.purchase(
            productId = "4005", 
            investData = [{
                "ccy":"USDT",
                "amt":"100"
            }]
        )
print(result)

```

#### 请求参数

| 参数         | 类型     | 是否必须 | 描述                                             |
|:-----------|:-------|:-----|:-----------------------------------------------|
| productId  | String | 是    | 项目ID                                           |
| investData | Array of objects  | 是    | 投资信息                                           |
| > ccy      | String | 是    | 投资币种，如 `BTC`                                   |
| > amt      | String | 是    | 投资数量                                           |
| term       | String | 可选   | 投资期限<br>定期项目必须指定投资期限                           |
| tag        | String | 否    | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间 |

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
