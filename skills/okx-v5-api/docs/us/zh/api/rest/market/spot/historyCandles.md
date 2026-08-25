---
site: us
lang: zh
name: GET / 获取交易产品历史K线数据
method: GET
path: /api/v5/market/history-candles
module: market
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### GET / 获取交易产品历史K线数据[order-book-trading-market-data-get-candlesticks-history]

获取最近几年的历史k线数据(1s k线支持查询最近3个月的数据)
#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/history-candles`

> 请求示例

```shell
GET /api/v5/market/history-candles?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取交易产品历史K线数据
result = marketDataAPI.get_history_candlesticks(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                                                                                         |
| :----- | :----- | :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId | String | 是       | 产品ID，如 `BTC-USDT`                                                                                                                                          |
| after  | String | 否       | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`ts`                                                                                                                         |
| before | String | 否       | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`ts`, 单独使用时，会返回最新的数据。                                                                                                        |
| bar    | String | 否       | 时间粒度，默认值`1m`<br>如 [1s/1m/3m/5m/15m/30m/1H/2H/4H] <br>UTC+8开盘价k线：[6H/12H/1D/2D/3D/1W/1M/3M]<br>UTC+0开盘价k线：[6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/1Wutc/1Mutc/3Mutc] |
| limit  | String | 否       | 分页返回的结果集数量，最大为100，不填默认返回100条                                                                                                                               |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
     [
        "1597026383085",
        "3.721",
        "3.743",
        "3.677",
        "3.708",
        "8422410",
        "22698348.04828491",
        "12698348.04828491",
        "1"
    ],
    [
        "1597026383085",
        "3.731",
        "3.799",
        "3.494",
        "3.72",
        "24912403",
        "67632347.24399722",
        "37632347.24399722",
        "1"
    ]
    ]
}
```

#### 返回参数

| **参数名**      | **类型**     | **描述**                                 |
|:-------------|:-----------|:---------------------------------------|
| ts           | String     | 开始时间，Unix时间戳的毫秒数格式，如  `1597026383085`  |
| o            | String     | 开盘价格                                   |
| h            | String     | 最高价格                                   |
| l            | String     | 最低价格                                   |
| c            | String     | 收盘价格                                   |
| vol          | String     | 交易量<br>如果是`币币`，数值为交易货币的数量。             |
| volCcy       | String     | 交易量<br>如果是`币币`，数值为计价货币的数量。             |
| volCcyQuote  | String     | 交易量，以计价货币为单位<br>如 `BTC-USDT`单位是`USDT`。 |
| confirm      | String     | K线状态<br>`0`：K线未完结<br>`1`：K线已完结         |

<aside class="notice">
返回值数组顺序分别为是：[ts,o,h,l,c,vol,volCcy,confirm]
</aside>

<aside class="notice">
期权不支持 1s K线， 其他业务线 (币币, 杠杆, 交割和永续)支持
</aside>
