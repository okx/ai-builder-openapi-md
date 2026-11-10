---
site: okex
lang: zh
name: GET / 获取所有产品行情信息
method: GET
path: /api/v5/market/tickers
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
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
GET /api/v5/market/tickers?instType=SWAP

```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取所有产品行情信息
result = marketDataAPI.get_tickers(
    instType="SWAP"
)
print(result)
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                      |
|:---------------| :----- | :------- |:------------------------------------------------------------------------|
| instType       | String | 是       | 产品类型<br>`SPOT`：币币<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| instFamily     | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`，如 `BTC-USD`                                       |                                                                                                                                 

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
        "instType":"SWAP",
        "instId":"LTC-USD-SWAP",
        "last":"9999.99",
        "lastSz":"1",
        "askPx":"9999.99",
        "askSz":"11",
        "bidPx":"8888.88",
        "bidSz":"5",
        "open24h":"9000",
        "high24h":"10000",
        "low24h":"8888.88",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "sodUtc0":"0.1",
        "sodUtc8":"0.1",
        "ts":"1597026383085"
     },
     {
        "instType":"SWAP",
        "instId":"BTC-USD-SWAP",
        "last":"9999.99",
        "lastSz":"1",
        "askPx":"9999.99",
        "askSz":"11",
        "bidPx":"8888.88",
        "bidSz":"5",
        "open24h":"9000",
        "high24h":"10000",
        "low24h":"8888.88",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "sodUtc0":"0.1",
        "sodUtc8":"0.1",
        "ts":"1597026383085"
    }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                |
| :--------- | :------- |:----------------------------------------------------------------------|
| instType   | String   | 产品类型                                                                  |
| instId     | String   | 产品ID                                                                  |
| last       | String   | 最新成交价                                                                 |
| lastSz     | String   | 最新成交的数量，0 代表没有成交量                                                     |
| askPx      | String   | 卖一价                                                                   |
| askSz      | String   | 卖一价的挂单数数量                                                             |
| bidPx      | String   | 买一价                                                                   |
| bidSz      | String    | 买一价的挂单数量                                                              |
| open24h    | String   | 24小时开盘价                                                               |
| high24h    | String   | 24小时最高价                                                               |
| low24h     | String   | 24小时最低价                                                               |
| volCcy24h  | String   | 24小时成交量，以`币`为单位<br>如果是`衍生品`合约，数值为交易货币的数量。比如，对于 BTC-USD-SWAP 和 BTC-USDT-SWAP，单位均为 BTC<br>如果是`币币/币币杠杆`，数值为计价货币的数量。 |
| vol24h     | String   | 24小时成交量，以`张`为单位<br>如果是`衍生品`合约，数值为合约的张数。<br>如果是`币币/币币杠杆`，数值为交易货币的数量。   |
| sodUtc0  | String      | UTC 0  时开盘价                                                           |
| sodUtc8     | String   | UTC+8 时开盘价                                                            |
| ts         | String   | ticker数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`                          |
