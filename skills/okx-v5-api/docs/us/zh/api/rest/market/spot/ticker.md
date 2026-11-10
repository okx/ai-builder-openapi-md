---
site: us
lang: zh
name: GET / 获取单个产品行情信息
method: GET
path: /api/v5/market/ticker
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### GET / 获取单个产品行情信息 [order-book-trading-market-data-get-ticker]

获取产品行情信息。在提前挂单阶段，best ask的价格有机会低于best bid。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/ticker`

> 请求示例

```shell
GET /api/v5/market/ticker?instId=BTC-USDT

```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取单个产品行情信息
result = marketDataAPI.get_ticker(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                          |
| :----- | :----- | :------- | :---------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USDT` |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "SPOT",
            "instId": "BTC-USDT",
            "last": "51240",
            "lastSz": "0.49011124",
            "askPx": "51240",
            "askSz": "0.64278176",
            "bidPx": "51239.9",
            "bidSz": "1.68139044",
            "open24h": "51695.6",
            "high24h": "52080",
            "low24h": "50936",
            "volCcy24h": "539533972.680195094",
            "vol24h": "10474.12353007",
            "ts": "1708669925904",
            "sodUtc0": "51290.1",
            "sodUtc8": "51602.4"
        }
    ]
}
```

#### 返回参数

| **参数名**    | **类型**     | **描述**                                       |
|:-----------|:-----------|:---------------------------------------------|
| instType   | String     | 产品类型                                         |
| instId     | String     | 产品ID                                         |
| last       | String     | 最新成交价                                        |
| lastSz     | String     | 最新成交的数量                                      |
| askPx      | String     | 卖一价                                          |
| askSz      | String     | 卖一价对应的数量                                     |
| bidPx      | String     | 买一价                                          |
| bidSz      | String     | 买一价对应的数量                                     |
| open24h    | String     | 24小时开盘价                                      |
| high24h    | String     | 24小时最高价                                      |
| low24h     | String     | 24小时最低价                                      |
| volCcy24h  | String     | 24小时成交量<br>如果是`币币`，数值为计价货币的数量。               |
| vol24h     | String     | 24小时成交量<br>如果是`币币`，数值为交易货币的数量。               |
| sodUtc0    | String     | UTC+0 时开盘价                                   |
| sodUtc8    | String     | UTC+8 时开盘价                                   |
| ts         | String     | ticker数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
