---
site: okex
lang: zh
name: 全部撤单
method: POST
path: /api/v5/sprd/mass-cancel
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 全部撤单 [spread-trading-rest-api-cancel-all-orders]

撤销所有挂单

#### 限速：10次/2s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/sprd/mass-cancel`

> 请求示例
> 

```shell
POST /api/v5/sprd/mass-cancel
 body
 {
    "sprdId": "BTC-USDT_BTC-USDT-SWAP"
}

```
```python
import okx.SpreadTrading as SpreadTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# 全部撤单
result = spreadAPI.cancel_all_orders(sprdId="BTC-USDT_BTC-USDT-SWAP")
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| sprdId | String | 否 | spread ID |

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| result | Boolean | 请求结果`true`, `false` |

> 返回示例
> 

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "result": true
        }
    ]
}

```

<aside class="notice">
返回结果中result=true 代表您的请求已被成功接收，并将会被处理。撤单的实际结果会通过`sprd-orders`频道推送。
</aside>
