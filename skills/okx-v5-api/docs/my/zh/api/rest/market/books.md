---
site: my
lang: zh
name: GET / 获取产品深度
method: GET
path: /api/v5/market/books
module: market
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取产品深度 [order-book-trading-market-data-get-order-book]

获取产品深度列表，数据每 50 毫秒更新一次。在提前挂单阶段，best ask的价格有机会低于best bid。<br>
该接口收到请求后不会立刻返回，而是会待服务端缓存数据更新后立即返回最新数据。

#### 限速：40次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/books`

> 请求示例

```shell
GET /api/v5/market/books?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取产品深度
result = marketDataAPI.get_orderbook(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                           |
| :----- | :----- | :------- | :--------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USDT`         |
| sz     | String | 否       | 深度档位数量，最大值可传400，即买卖深度共800条 <br>不填写此参数，默认返回`1`档深度数据|

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "41006.8",
                    "0.60038921",
                    "0",
                    "1"
                ]
            ],
            "bids": [
                [
                    "41006.3",
                    "0.30178218",
                    "0",
                    "2"
                ]
            ],
            "ts": "1629966436396",
            "seqId": 3235851742
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**       |
| :--------- | :------- | :------------- |
| asks       | Array of Arrays | 卖方深度       |
| bids       | Array of Arrays | 买方深度       |
| ts         | String   | 深度产生的时间 |
| seqId      | Integer  | 当前消息的序列号 |

<aside class="notice">
合约的asks和bids值数组举例说明： ["411.8","10", "0","4"] 411.8为深度价格，10为此价格的合约张数，0该字段已弃用(始终为0)，4为此价格的订单数量<br>
现货/币币杠杆的asks和bids值数组举例说明： ["411.8","10", "0","4"] 411.8为深度价格，10为此价格的交易币的数量，0该字段已弃用(始终为0)，4为此价格的订单数量
asks和bids值数组举例说明： ["411.8", "10", "0", "4"]<br>
- 411.8为深度价格<br>
- 10为此价格的数量 （合约交易为张数，现货/币币杠杆为交易币的数量）<br>
- 0该字段已弃用(始终为0)<br>
- 4为此价格的订单数量
</aside>

<aside class="notice">
集合竞价期间，深度数据大约每秒更新一次
</aside>
