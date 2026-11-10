---
site: okex
lang: zh
name: GET / 获取交易产品K线数据
method: GET
path: /api/v5/market/candles
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取交易产品K线数据 [order-book-trading-market-data-get-candlesticks]

获取K线数据。K线数据按请求的粒度分组返回，K线数据每个粒度最多可获取最近1,440条。

#### 限速：40次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/candles`

> 请求示例

```shell
GET /api/v5/market/candles?instId=BTC-USDT
```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取交易产品K线数据
result = marketDataAPI.get_candlesticks(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                                                                                                                       |
| :----- | :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId | String | 是       | 产品ID，如 `BTC-USDT`                                                                                                                                        |
| bar    | String | 否       | 时间粒度，默认值`1m`<br>如 [1m/3m/5m/15m/30m/1H/2H/4H] <br>UTC+8开盘价k线：[6H/12H/1D/2D/3D/1W/1M/3M]<br>UTC+0开盘价k线：[/6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/1Wutc/1Mutc/3Mutc] |
| after  | String | 否       | 请求此时间戳之前（更旧的数据）的分页内容，传的值为对应接口的`ts`                                                                                                                       |
| before | String | 否       | 请求此时间戳之后（更新的数据）的分页内容，传的值为对应接口的`ts`, 单独使用时，会返回最新的数据。                                                                                                      |
| limit  | String | 否       | 分页返回的结果集数量，最大为300，不填默认返回100条                                                                                                                             |
| adjust | String | 否       | 复权类型，仅适用于股票永续合约。<br>`forward`：前复权。<br>不填时默认返回不复权数据。 |

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
        "0"
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

| **参数名**      | **类型** | **描述**                                                                              |
|:-------------| :------- |:------------------------------------------------------------------------------------|
| ts           | String   | 开始时间，Unix时间戳的毫秒数格式，如 `1597026383085`                                                |
| o            | String   | 开盘价格                                                                                |
| h            | String   | 最高价格                                                                                |
| l            | String   | 最低价格                                                                                |
| c            | String   | 收盘价格                                                                                |
| vol          | String   | 交易量，以`张`为单位<br>如果是`衍生品`合约，数值为合约的张数。<br>如果是`币币/币币杠杆`，数值为交易货币的数量。                     |
| volCcy       | String   | 交易量，以`币`为单位<br>如果是`衍生品`合约，数值为交易货币的数量。<br>如果是`币币/币币杠杆`，数值为计价货币的数量。                   |
| volCcyQuote  | String  | 交易量，以计价货币为单位<br>如 `BTC-USDT`和`BTC-USDT-SWAP`，单位均是`USDT`。<br>`BTC-USD-SWAP`单位是`USD`。 |
| confirm      | String  | K线状态<br>`0`：K线未完结<br>`1`：K线已完结                                                      |

<aside class="notice">
返回的第一条K线数据可能不是完整周期k线，返回值数组顺序分别为是：[ts,o,h,l,c,vol,volCcy,volCcyQuote,confirm] <br>
对于当前周期的K线数据，没有成交时，开高收低默认都取上一周期的收盘价格。
</aside>

<aside class="notice">
当传入 <code>adjust=forward</code> 时，历史K线的开高低收（OHLC）价格将乘以对应时期的复权因子。对于拆股，成交量（<code>vol</code>、<code>volCcy</code>）也会按相同比例调整。成交金额（<code>volCcyQuote</code>）不做调整。该参数仅对股票永续合约有效。
</aside>
