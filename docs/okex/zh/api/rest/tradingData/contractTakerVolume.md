---
site: okex
lang: zh
name: 获取合约主动买入/卖出情况
method: GET
path: /api/v5/rubik/stat/taker-volume-contract
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取合约主动买入/卖出情况 [trading-statistics-rest-api-get-contract-taker-volume]

获取合约维度taker主动买入和卖出的交易量。每个粒度最多可获取最近1,440条数据。<br>

对于时间粒度period=1D，数据时间范围最早至2024年1月1日；对于其他时间粒度period，最早至2024年2月初。

#### 限速： 5次/2s
#### 限速规则： IP + Instrument ID
#### HTTP请求

`GET /api/v5/rubik/stat/taker-volume-contract`

> 请求示例

```shell
GET /api/v5/rubik/stat/taker-volume-contract?instId=BTC-USDT-SWAP
```

```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取合约taker主动买入和卖出的交易量
result = tradingDataAPI.get_contract_taker_volume(
    instId="BTC-USDT-SWAP"
)

print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| instId | string | 是 | 产品ID，如 BTC-USDT <br>仅适用于`交割`/`永续` |
| period | string | 否 | 时间粒度，默认值`5m`, 如 [`5m/15m/30m/1H/2H/4H`] <br>UTC+8开盘价k线：[`6H/12H/1D/2D/3D/5D/1W/1M/3M`] <br>UTC+0开盘价k线： [`6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/5Dutc/1Wutc/1Mutc/3Mutc`] |
| unit | string | 否 | 买入、卖出的单位，默认值是`1` <br>`0`: 币 <br>`1`: 合约 <br>`2`: U |
| end | string | 否 | 筛选的结束时间戳 ts，Unix 时间戳为毫秒数格式，如 `1597027383085` |
| begin | string | 否 | 筛选的开始时间戳 ts，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| limit | string | 否 | 分页返回的结果集数量，最大为100，不填默认返回100条 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "1701417600000",    // timestamp
            "200",              // taker sell volume
            "380"               // taker buy volume
        ],
        [
            "1701417600000",    // timestamp
            "100",              // taker sell volume
            "300"               // taker buy volume
        ]
    ]
}

```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| ts | String | 时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` |
| sellVol | String | 卖出量 |
| buyVol | String | 买入量 |

返回值数组顺序分别为是：[ts, sellVol, buyVol]
