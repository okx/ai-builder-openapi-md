---
site: hongkong
lang: zh
name: GET / 获取所有产品行情信息
method: GET
path: /api/v5/market/tickers
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取所有产品行情信息 [order-book-trading-market-data-get-tickers]

获取产品行情信息。在提前挂单阶段，best ask的价格有机会低于best bid。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/tickers`

> 请求示例

```shell
GET /api/v5/market/tickers?instType=SPOT

```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取所有产品行情信息
result = marketDataAPI.get_tickers(
    instType="SPOT"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                      |
|:---------------| :----- | :------- |:------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`SPOT`：币币 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "instType": "SPOT",
            "instId": "BTC-USDT",
            "last": "51230",
            "lastSz": "0.18531491",
            "askPx": "51229.4",
            "askSz": "2.1683067",
            "bidPx": "51229.3",
            "bidSz": "0.28249897",
            "open24h": "51635.7",
            "high24h": "52080",
            "low24h": "50936",
            "volCcy24h": "539658490.410419122",
            "vol24h": "10476.2229261",
            "ts": "1708669508505",
            "sodUtc0": "51290.1",
            "sodUtc8": "51602.4"
        }
    ]
}
```

#### 返回参数

| **参数名**      | **类型**     | **描述**                                                                |
|:-------------|:-----------|:----------------------------------------------------------------------|
| instType     | String     | 产品类型                                                                  |
| instId       | String     | 产品ID                                                                  |
| last         | String     | 最新成交价                                                                 |
| lastSz       | String     | 最新成交的数量                                                               |
| askPx        | String     | 卖一价                                                                   |
| askSz        | String     | 卖一价的挂单数数量                                                             |
| bidPx        | String     | 买一价                                                                   |
| bidSz        | String     | 买一价的挂单数量                                                              |
| open24h      | String     | 24小时开盘价                                                               |
| high24h      | String     | 24小时最高价                                                               |
| low24h       | String     | 24小时最低价                                                               |
| volCcy24h    | String     | 24小时成交量<br>如果是`币币`，数值为计价货币的数量。 |
| vol24h       | String     | 24小时成交量<br>如果是`币币`，数值为交易货币的数量。   |
| sodUtc0      | String     | UTC 0  时开盘价                                                           |
| sodUtc8      | String     | UTC+8 时开盘价                                                            |
| ts           | String     | ticker数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                          |
