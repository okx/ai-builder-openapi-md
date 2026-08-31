---
site: okex
lang: zh
name: 获取合约持仓量历史
method: GET
path: /api/v5/rubik/stat/contracts/open-interest-history
module: rubik
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取合约持仓量历史 [trading-statistics-rest-api-get-contract-open-interest-history]

获取交割及永续合约的历史持仓量数据。每个粒度最多可获取最近1,440条数据。<br>

对于时间粒度period=1D，数据时间范围最早至2024年1月1日；对于其他时间粒度period，最早至2024年2月初。

#### 限速：10次/2s

#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/rubik/stat/contracts/open-interest-history`

> 请求示例

```shell
GET /api/v5/rubik/stat/contracts/open-interest-history?instId=BTC-USDT-SWAP

```

```python
import okx.TradingData as TradingData_api

flag = "0"  # 实盘: 0, 模拟盘: 1

tradingDataAPI = TradingData_api.TradingDataAPI(flag=flag)

# 获取持仓量历史
result = tradingDataAPI.get_open_interest_history(
    instId="BTC-USDT-SWAP"
)

print(result)
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
| --- | --- | --- | --- |
| instId | string | 是 | 产品ID，如 BTC-USDT-SWAP <br>仅适用于`交割`/`永续` |
| period | string | 否 | 时间粒度，默认值`5m`, 如 [`5m/15m/30m/1H/2H/4H`] <br>UTC+8开盘价k线：[`6H/12H/1D/2D/3D/5D/1W/1M/3M`] <br>UTC+0开盘价k线： [`6Hutc/12Hutc/1Dutc/2Dutc/3Dutc/5Dutc/1Wutc/1Mutc/3Mutc`] |
| end | string | 否 | 筛选的结束时间戳 ts，Unix 时间戳为毫秒数格式，如 `1597027383085` |
| begin | string | 否 | 筛选的开始时间戳 ts，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| limit | string | 否 | 分页返回的结果集数量，最大为`100`，不填默认返回`100`条 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        [
            "1701417600000",    // timestamp
            "731377.57500501",   // open interest (oi, contracts)
            "111",              // open interest (oiCcy, coin)
            "8888888"         // open interest (oiUsd, USD)
        ],
        [
            "1701417500000",    // timestamp
            "731377.57500501",   // open interest (oi, contracts)
            "111",              // open interest (oiCcy, coin)
            "8888888"         // open interest (oiUsd, USD)
        ]
    ]
}

```

#### 返回参数

| 参数名 | 类型 | 描述 |
| --- | --- | --- |
| ts | String | 时间戳，Unix时间戳的毫秒数格式，如 `1597026383085` |
| oi | String | 合约单位的持仓量 |
| oiCcy | String | 币种单位的持仓量 |
| oiUsd | String | USD单位的持仓量 |

返回值数组顺序分别为是：[ts, oi, oiCcy, oiUsd]
