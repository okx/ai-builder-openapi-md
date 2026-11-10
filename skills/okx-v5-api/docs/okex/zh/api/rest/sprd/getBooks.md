---
site: okex
lang: zh
name: 获取Spread产品深度（公共）
method: GET
path: /api/v5/sprd/books
module: sprd
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取Spread产品深度（公共） [spread-trading-rest-api-get-order-book-public]

获取Spread产品深度列表

#### 限速：20次/2s

#### 限速规则：IP

#### HTTP请求

`GET /api/v5/sprd/books`

> 请求示例
> 

```shell
GET /api/v5/sprd/books?sprdId=BTC-USDT_BTC-USDT-SWAP

```
```python
import okx.SpreadTrading as SpreadTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

spreadAPI = SpreadTrading.SpreadTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取深度
result = spreadAPI.get_order_book(sprdId="BTC-USDT_BTC-USDT-SWAP", sz=20)
print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| sprdId | String | 是 | spread ID，如BTC-USDT_BTC-USDT-SWAP |
| sz | String | 否 | 深度档位数量。最大值为400。默认值为5。 |

> 返回示例
> 

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "41006.8", // 价格
                    "0.60038921", // 数量
                    "1" // 此价格上订单数量
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "2"
                ]
            ],
            "ts": "1629966436396"
        }
    ]
}

```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| asks | Array of Arrays | 卖方深度 |
| bids | Array of Arrays | 买方深度 |
| ts | String | 深度产生的时间 |

<aside class="notice">
asks和bids值数组举例说明： ["411.8", "10", "4"] <br>
- 411.8为深度价格 <br>
- 10为此价格的数量 (单位为szCcy）<br>
- 4为此价格的订单数量 <br>
</aside>
