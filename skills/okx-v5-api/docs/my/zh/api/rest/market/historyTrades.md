---
site: my
lang: zh
name: GET / 获取交易产品公共历史成交数据
method: GET
path: /api/v5/market/history-trades
module: market
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### GET / 获取交易产品公共历史成交数据 [order-book-trading-market-data-get-trades-history]

查询市场上的成交信息数据，可以分页获取最近3个月的数据。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/history-trades`

> 请求示例

```shell
GET /api/v5/market/history-trades?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取交易产品公共历史成交数据
result = marketDataAPI.get_history_trades(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名    | 类型   | 是否必须 | 描述                                                                             |
|:-------| :----- |:-----|:-------------------------------------------------------------------------------|
| instId | String | 是    | 产品ID，如 `BTC-USDT`                                                              |
| type   | String | 否    | 分页类型<br>`1`：tradeId 分页 `2`：时间戳分页<br>默认为`1`：tradeId 分页         |
| after  | String | 否    | 请求此 ID 或 ts 之前的分页内容，传的值为对应接口的 tradeId 或 ts                        |
| before | String | 否    | 请求此ID之后（更新的数据）的分页内容，传的值为对应接口的 tradeId。<br>不支持时间戳分页。单独使用时，会返回最新的数据。                   |
| limit  | String | 否    | 分页返回的结果集数量，最大为100，不填默认返回100条                                                   |

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

| **参数名** | **类型** | **描述**                                             |
| :--------- | :------- | :--------------------------------------------------- |
| instId     | String   | 产品ID                                               |
| tradeId    | String   | 成交ID                                               |
| px         | String   | 成交价格                                             |
| sz         | String   | 成交数量<br>对于币币交易，成交数量的单位为交易货币<br/>对于交割、永续以及期权，单位为张。 |
| side       | String   | 吃单方向 <br>`buy`：买  <br>`sell`：卖                 |
| source | String | 订单来源<br> `0`：普通订单<br> `1`：流动性增强计划订单 |
| ts         | String   | 成交时间，Unix时间戳的毫秒数格式， 如`1597026383085` |
