---
site: us
lang: zh
name: GET / 获取交易产品公共成交数据
method: GET
path: /api/v5/market/trades
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / 获取交易产品公共成交数据 [order-book-trading-market-data-get-trades]

查询市场上的成交信息数据

#### 限速：100次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/trades`

> 请求示例

```shell
GET /api/v5/market/trades?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取交易产品公共成交数据
result = marketDataAPI.get_trades(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USDT`                      |
| limit  | String | 否       | 分页返回的结果集数量，最大为500，不填默认返回100条 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "source": "0",
            "px": "29963.2",
            "tradeId": "242720720",
            "ts": "1654161646974"
        },
        {
            "instId": "BTC-USDT",
            "side": "sell",
            "sz": "0.00001",
            "source": "0",
            "px": "29964.1",
            "tradeId": "242720719",
            "ts": "1654161641568"
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                            |
| :--------- | :------- |:--------------------------------------------------|
| instId     | String   | 产品ID                                              |
| tradeId    | String   | 成交ID                                              |
| px         | String   | 成交价格                                              |
| sz         | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| side       | String   | 吃单方向<br>`buy`：买<br>`sell`：卖                       |
| source | String | 订单来源<br> `0`：普通订单<br> `1`：RPI 订单 |
| ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如`1597026383085`              |

<aside class="notice">
最多获取最近500条历史公共成交数据
</aside>
