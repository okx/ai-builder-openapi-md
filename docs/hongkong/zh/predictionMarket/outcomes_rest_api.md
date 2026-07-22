---
site: hongkong
lang: zh
name: 1.1 获取事件列表
method: GET
path: /api/v5/predictions/events
module: predictions
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
multi_endpoint: true
---

# REST API [rest-api]

REST API 用于查询事件、市场、行情、订单簿、订单、成交、持仓、余额，以及提交下单、撤单、Split、Merge、Redeem 等写操作。开发者可以通过 REST API 完成完整的交易闭环：发现市场、获取 `assetId`、提交订单、查询订单状态、管理持仓并处理结算相关操作。所有接口的基础地址为 `https://www.okx.com`。

## 1. 事件与市场 [rest-api-1-events-markets-api]

**数据结构定义**

**结果选项 OutcomeResp**

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| tokenId | string / null | 条件代币合约地址；未上链前为 null |
| assetId | string / null | TradeZone 资产 ID（下单时使用）；未上链前为 null |
| name | string | 结果名称，如 `"Yes"`、`"No"` |
| price | string | 当前价格（0–1 之间的字符串，如 "0.82"） |
| bgColor | string | 按钮背景色（Hex）；体育 moneyline Yes outcome 取主队主题色；其余为 null |

**市场对象 MarketResp**

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | string | 市场中心化唯一 ID |
| marketId | string | 市场在 TradeZone 中的唯一 ID（下单时使用） |
| negRisk | boolean | 是否互斥市场（negRisk） |
| status | string | 市场状态：`active` / `paused` / `settling` / `resolved` |
| settleStage | int | 结算阶段：0=未开始 1=第一轮公示 2=第一轮 dispute 3=第二轮公示 4=第二轮 dispute 5=已结算 |
| question | string | 完整市场问题 |
| shortQuestion | string / null | 缩略市场问题 |
| description | string | 市场描述 |
| marketIcon | string / null | 市场图标 URL |
| bestBid | string / null | 最优买价（0–1）；无买盘时为 null |
| bestAsk | string / null | 最优卖价（0–1）；无卖盘时为 null |
| lastTradePrice | string / null | 最新成交价（0–1）；从未成交时为 null |
| volume | string | 总交易量（xp） |
| probability | string / null | 市场 Yes 概率（0–1 小数）；未上链时为 null |
| resolutionSources | string[] | 结算数据来源 URL 列表 |
| yesOutcome | OutcomeObject | Yes 方结果选项 |
| noOutcome | OutcomeObject | No 方结果选项 |
| startTime | string | market 预计开始时间（毫秒时间戳） |
| endTime | string | market 预计结束时间（毫秒时间戳） |
| resolveStartAt | string | 首次进入 start_resolve 状态的时间（毫秒时间戳） |
| resolveAt | string | 首次进入 resolved 状态的时间（毫秒时间戳） |

**事件对象 EventResp**

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | string | 事件中心化唯一 ID |
| eventId | string | 事件在 TradeZone 中的唯一 ID |
| negRisk | boolean | 是否负风险事件 |
| status | string | 事件状态：`active` / `paused` / `resolved` |
| eventTitle | string | 事件标题 |
| description | string | 事件描述 |
| eventIcon | string / null | 事件图标 URL |
| volume | string | 事件内所有市场交易量之和（xp） |
| startTime | string / null | 开始交易时间戳（ms） |
| endTime | string / null | 停止交易时间戳（ms） |
| createdAt | string | 事件创建时间戳（ms） |
| totalMarketsCount | int | 事件下市场总数 |
| finalOutcomesMarketId | string / null | 结算后胜出市场 ID；未结算时为 null |
| markets | array\<MarketResp\> | 市场列表（列表接口最多返回前 2 个；完整列表通过 `GET /api/v5/predictions/events/{eventId}/markets` 获取） |

**游标分页 PaginationResp**

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| nextCursor | string / null | 下一页游标；null 表示已是最后一页 |
| hasMore | boolean | 是否有更多数据 |
| pageSize | int | 本次返回数量 |

### 1.1 获取事件列表 [rest-api-1-events-markets-api-1-1-get-events]

获取预测市场事件列表，支持状态等多维过滤与排序。

#### HTTP请求 [rest-api-1-events-markets-api-1-1-get-events-http-request]

`GET /api/v5/predictions/events`

#### 请求参数（Query） [rest-api-1-events-markets-api-1-1-get-events-request-parameters-query]

| 参数 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- | --- | --- | --- |
| status | string | 否 | `active` | 事件状态过滤：`active` / `resolved` |
| sort | string | 否 | `volume_24h` | 排序方式：`volume` / `volume_24h` / `ending_soon` / `newest` |
| tag | string | 否 | - | 体育标签 ID 过滤（来自 GET /api/v5/predictions/sports/tags） |
| leagueId | string | 否 | - | 联赛 ID 过滤（来自 GET /api/v5/predictions/sports/tags/{tagId}/leagues） |
| cursor | string | 否 | - | 分页游标，首次请求不传 |
| pageSize | int | 否 | 10 | 每页条数（最大 50） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "events": [
      {
        "id": "100",
        "eventId": "100",
        "negRisk": false,
        "status": "active",
        "eventTitle": "Will BTC exceed $100k by end of 2026?",
        "description": "This event tracks whether Bitcoin will cross $100,000.",
        "eventIcon": "https://cdn.example.com/events/100.png",
        "volume": "1250000.00",
        "startTime": 1700000000000,
        "endTime": 1798761600000,
        "createdAt": 1710000000000,
        "totalMarketsCount": 1,
        "finalOutcomesMarketId": null,
        "markets": [
          {
            "id": "1",
            "marketId": "1",
            "oddsType": "points",
            "negRisk": false,
            "status": "active",
            "settleStage": 0,
            "question": "Will BTC exceed $100k by end of 2026?",
            "shortQuestion": "BTC > $100k?",
            "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
            "marketIcon": null,
            "bestBid": "0.64",
            "bestAsk": "0.66",
            "lastTradePrice": "0.65",
            "volume": "1250000.00",
            "probability": "0.65",
            "resolutionSources": ["https://coinmarketcap.com"],
            "yesOutcome": {
              "tokenId": "0xabc...001",
              "assetId": "1",
              "name": "Yes",
              "price": "0.65",
              "finalResult": null
            },
            "noOutcome": {
              "tokenId": "0xabc...002",
              "assetId": "2",
              "name": "No",
              "price": "0.35",
              "finalResult": null
            }
          }
        ]
      }
    ],
    "pagination": {
      "nextCursor": "eyJpZCI6MTAwfQ",
      "hasMore": true,
      "pageSize": 10
    }
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| events | array\<EventResp\> | 事件列表；每个事件的 `markets` 最多返回前 2 个市场 |
| pagination | PaginationResp | 游标分页信息 |

### 1.2 获取单个事件 [rest-api-1-events-markets-api-1-2-get-a-single-event]

获取指定事件的完整信息，`markets` 字段返回该事件下所有市场（不截断）。

#### HTTP请求 [rest-api-1-events-markets-api-1-2-get-a-single-event-http-request]

`GET /api/v5/predictions/events/{eventId}`

#### 路径参数 [rest-api-1-events-markets-api-1-2-get-a-single-event-path-parameters]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| eventId | string | 是 | 事件 ID |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "100",
    "eventId": "100",
    "negRisk": false,
    "status": "active",
    "eventTitle": "NBA Finals 2026 - Lakers vs Celtics",
    "description": "Who will win the 2026 NBA Championship?",
    "eventIcon": "https://cdn.example.com/events/100.png",
    "volume": "3200000.00",
    "startTime": 1748000000000,
    "endTime": 1750000000000,
    "createdAt": 1710000000000,
    "totalMarketsCount": 3,
    "finalOutcomesMarketId": null,
    "markets": [ ]
  }
}
```

返回完整 EventResp，`markets` 包含该事件下的全部市场。

### 1.3 获取市场列表 [rest-api-1-events-markets-api-1-3-get-markets]

获取指定事件下的全部市场，不分页。

#### HTTP请求 [rest-api-1-events-markets-api-1-3-get-markets-http-request]

`GET /api/v5/predictions/events/{eventId}/markets`

#### 路径参数 [rest-api-1-events-markets-api-1-3-get-markets-path-parameters]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| eventId | string | 是 | 事件 ID |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "markets": [
      {
        "id": "1",
        "marketId": "1",
        "negRisk": false,
        "status": "active",
        "settleStage": 0,
        "question": "Will BTC exceed $100k by end of 2026?",
        "shortQuestion": "BTC > $100k?",
        "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
        "marketIcon": null,
        "bestBid": "0.64",
        "bestAsk": "0.66",
        "lastTradePrice": "0.65",
        "volume": "1250000.00",
        "probability": "0.65",
        "resolutionSources": ["https://coinmarketcap.com"],
        "yesOutcome": {
          "tokenId": "0xabc...001",
          "assetId": "1",
          "name": "Yes",
          "price": "0.65",
          "finalResult": null
        },
        "noOutcome": {
          "tokenId": "0xabc...002",
          "assetId": "2",
          "name": "No",
          "price": "0.35",
          "finalResult": null
        }
      }
    ]
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| markets | array\<MarketResp\> | 该事件下的全部市场 |

### 1.4 获取单个市场 [rest-api-1-events-markets-api-1-4-get-a-single-market]

获取指定市场的详细信息。

#### HTTP请求 [rest-api-1-events-markets-api-1-4-get-a-single-market-http-request]

`GET /api/v5/predictions/markets/{marketId}`

#### 路径参数 [rest-api-1-events-markets-api-1-4-get-a-single-market-path-parameters]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| marketId | string | 是 | 市场 TradeZone ID |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "1",
    "marketId": "1",
    "negRisk": false,
    "status": "resolved",
    "settleStage": 5,
    "question": "Will BTC exceed $100k by end of 2026?",
    "shortQuestion": "BTC > $100k?",
    "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
    "marketIcon": null,
    "bestBid": null,
    "bestAsk": null,
    "lastTradePrice": "0.98",
    "volume": "1250000.00",
    "probability": null,
    "resolutionSources": ["https://coinmarketcap.com"],
    "yesOutcome": {
      "tokenId": "0xabc...001",
      "assetId": "1",
      "name": "Yes",
      "price": "1.00",
      "finalResult": true
    },
    "noOutcome": {
      "tokenId": "0xabc...002",
      "assetId": "2",
      "name": "No",
      "price": "0.00",
      "finalResult": false
    }
  }
}
```

返回完整 MarketResp。

### 1.5 搜索事件 [rest-api-1-events-markets-api-1-5-search-events]

按关键字全文搜索事件，返回匹配的事件列表，支持游标分页。

#### HTTP请求 [rest-api-1-events-markets-api-1-5-search-events-http-request]

`GET /api/v5/predictions/events/search`

#### 请求参数（Query） [rest-api-1-events-markets-api-1-5-search-events-request-parameters-query]

| 参数 | 类型 | 必填 | 默认值 | 说明 |
| --- | --- | --- | --- | --- |
| keyword | string | 是 | - | 搜索关键字，匹配事件标题和描述 |
| cursor | string | 否 | - | 分页游标，首次请求不传 |
| pageSize | int | 否 | 10 | 每页条数（最大 50） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "events": [
      {
        "id": "100",
        "eventId": "100",
        "negRisk": false,
        "status": "active",
        "eventTitle": "Will BTC exceed $100k by end of 2026?",
        "description": "This event tracks whether Bitcoin will cross $100,000.",
        "eventIcon": "https://cdn.example.com/events/100.png",
        "volume": "1250000.00",
        "startTime": 1700000000000,
        "endTime": 1798761600000,
        "createdAt": 1710000000000,
        "totalMarketsCount": 1,
        "finalOutcomesMarketId": null,
        "markets": [
          {
            "id": "1",
            "marketId": "1",
            "oddsType": "points",
            "negRisk": false,
            "status": "active",
            "settleStage": 0,
            "question": "Will BTC exceed $100k by end of 2026?",
            "shortQuestion": "BTC > $100k?",
            "description": "Resolves YES if BTC price exceeds $100,000 before Jan 1, 2027.",
            "marketIcon": null,
            "bestBid": "0.64",
            "bestAsk": "0.66",
            "lastTradePrice": "0.65",
            "volume": "1250000.00",
            "probability": "0.65",
            "resolutionSources": ["https://coinmarketcap.com"],
            "yesOutcome": {
              "tokenId": "0xabc...001",
              "assetId": "1",
              "name": "Yes",
              "price": "0.65",
              "finalResult": null
            },
            "noOutcome": {
              "tokenId": "0xabc...002",
              "assetId": "2",
              "name": "No",
              "price": "0.35",
              "finalResult": null
            }
          }
        ]
      }
    ],
    "pagination": {
      "nextCursor": "eyJpZCI6MTAwfQ",
      "hasMore": true,
      "pageSize": 10
    }
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| events | array\<EventResp\> | 事件列表 |
| pagination | PaginationResp | 游标分页信息 |

## 2. 价格 [rest-api-2-price-api]

<aside class="notice">
<b>说明：</b> 所有接口中的 <code>instId</code> 参数均取预测市场的 <code>yesAssetId</code>，<code>yesAssetId</code> 由 prediction-market 后端进行提供。
</aside>

### 2.1 获取行情 Ticker [rest-api-2-price-api-2-1-get-ticker]

获取单个预测市场产品的最新行情信息，用于订单簿中的最新成交价展示

#### 请求参数 [rest-api-2-price-api-2-1-get-ticker-request-parameters]

| 参数 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| `instId` | String | 是 | 预测市场 yesAssetId |

#### HTTP请求 [rest-api-2-price-api-2-1-get-ticker-http-request]

`GET /api/v5/market/ticker?instId={yesAssetId}`

#### 返回参数 [rest-api-2-price-api-2-1-get-ticker-response-parameters]

> 返回示例

```json
{
  "code": "0",
  "msg": "",
  "data": [{
    "instType": "SPOT",
    "instId": "{yesAssetId}",
    "last": "0.65",
    "lastSz": "100",
    "askPx": "0.66",
    "askSz": "500",
    "bidPx": "0.64",
    "bidSz": "300",
    "open24h": "0.60",
    "high24h": "0.70",
    "low24h": "0.55",
    "vol24h": "10000",
    "volCcy24h": "6500",
    "sodUtc0": "0.62",
    "sodUtc8": "0.61",
    "ts": "1711900800000"
  }]
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `instType` | String | 产品类型 |
| `instId` | String | 产品ID |
| `last` | String | 最新成交价 |
| `lastSz` | String | 最新成交数量 |
| `askPx` | String | 卖一价 |
| `askSz` | String | 卖一价对应数量 |
| `bidPx` | String | 买一价 |
| `bidSz` | String | 买一价对应数量 |
| `open24h` | String | 24小时开盘价 |
| `high24h` | String | 24小时最高价 |
| `low24h` | String | 24小时最低价 |
| `vol24h` | String | 24小时成交量（以张计） |
| `volCcy24h` | String | 24小时成交量（以计价货币计） |
| `sodUtc0` | String | UTC 0 时开盘价 |
| `sodUtc8` | String | UTC\+8 时开盘价 |
| `ts` | String | 数据更新时间（Unix 毫秒时间戳） |

### 2.2 获取 K 线数据 [rest-api-2-price-api-2-2-get-candlesticks]

查询预测市场产品的历史 K 线数据，K线数据按请求的粒度分组返回，K线数据每个粒度最多可获取最近1,440条

#### 请求参数 [rest-api-2-price-api-2-2-get-candlesticks-request-parameters]

| 参数 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| `instId` | String | 是 | 预测市场 yesAssetId |
| `bar` | String | 否 | K 线周期，默认 `1m`。可选值：`1m` `3m` `5m` `15m` `30m` `1H` `2H` `4H` `6H` `12H` `1D` `1W` `1M` |
| `after` | String | 否 | 分页游标，返回该时间戳之前的数据（Unix 毫秒） |
| `before` | String | 否 | 分页游标，返回该时间戳之后的数据（Unix 毫秒） |
| `limit` | String | 否 | 每页数量，最大 100，默认 100 |

#### HTTP请求 [rest-api-2-price-api-2-2-get-candlesticks-http-request]

`GET /api/v5/market/candles?instId={yesAssetId}&bar=1m&limit=100`

**返回数据**

（data 为二维数组，每条记录字段顺序如下）

> 返回示例

```json
{
  "code": "0",
  "msg": "",
  "data": [
    ["1711900800000", "0.65", "0.67", "0.63", "0.66", "2000", "1300", "1300", "1"],
    ["1711900740000", "0.63", "0.66", "0.62", "0.65", "1800", "1170", "1170", "1"]
  ]
}
```

| 索引 | 字段 | 类型 | 说明 |
| --- | --- | --- | --- |
| 0 | `ts` | String | K 线开始时间（Unix 毫秒时间戳） |
| 1 | `o` | String | 开盘价 |
| 2 | `h` | String | 最高价 |
| 3 | `l` | String | 最低价 |
| 4 | `c` | String | 收盘价 |
| 5 | `vol` | String | 成交量（以张计） |
| 6 | `volCcy` | String | 成交量（以计价货币计） |
| 7 | `volCcyQuote` | String | 成交量（以报价货币计） |
| 8 | `confirm` | String | K 线状态：`0` 未确认，`1` 已确认 |

<aside class="notice">
<b>分页说明：</b> 数据按时间倒序返回。如需翻页，取最后一条记录的 <code>ts</code> 值作为下次请求的 <code>after</code> 参数。
</aside>

### 2.3 获取深度数据 [rest-api-2-price-api-2-3-get-order-book]

查询预测市场产品的买卖盘口深度快照

#### 请求参数 [rest-api-2-price-api-2-3-get-order-book-request-parameters]

| 参数 | 类型 | 必须 | 说明 |
| --- | --- | --- | --- |
| `instId` | String | 是 | 预测市场 yesAssetId |
| `sz` | String | 否 | 深度档位数量，最大值可传400，即买卖深度共800条 不填写此参数，默认返回`1`档深度数据 |

#### HTTP请求 [rest-api-2-price-api-2-3-get-order-book-http-request]

`GET /api/v5/market/pm-books?instId={yesAssetId}&sz=400`

#### 返回参数 [rest-api-2-price-api-2-3-get-order-book-response-parameters]

> 返回示例

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "asks": [
        ["67364.1","0.45478048","5"]
      ],
      "bids": [
        ["67364","1.72315936", "17"]
      ],
      "ts": "1774943488756",
      "seqId": 74487243135
    }
  ]
}

```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `asks` | Array | 卖盘数据，按价格**从低到高**排列 |
| `bids` | Array | 买盘数据，按价格**从高到低**排列 |
| `ts` | String | 深度快照时间（Unix 毫秒时间戳） |
| `seqId` | Number | 订单簿的版本号，预测市场无需关心 |

`asks` / `bids` 数组中每条记录格式：`[价格, 数量, 订单数量]`

## 3. 订单 [rest-api-3-orders-api]

### 3.1 下单 [rest-api-3-orders-api-3-1-place-order]

提交签名后的下单请求。开发者自行构造 calldata 并用自己的钱包私钥（ECDSA）签名，服务端校验后写入数据库并直接转发至 TradeZone（不经过 AA Wallet）。

#### HTTP请求 [rest-api-3-orders-api-3-1-place-order-http-request]

`POST /api/v5/predictions/orders`

#### 请求体 [rest-api-3-orders-api-3-1-place-order-request-body]

> 请求示例

```json
{
  "action": {
    "type": "placeOrder",
    "grouping": "na",
    "orders": [{
      "assetId": "1",
      "marketType": "prediction",
      "side": "buy",
      "price": "0.65",
      "size": "100",
      "reduceOnly": false,
      "clientOrderId": "0x0197a98c91312671ca83f15ccbd5186f",
      "orderType": { "limit": { "tif": "gtc" } }
    }]
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | 下单动作 |
| action.type | string | 是 | 固定 `"placeOrder"` |
| action.grouping | string | 是 | 固定 `"na"` |
| action.orders | array | 是 | 订单列表（每次一笔） |
| action.orders[].assetId | string | 是 | TradeZone 资产 ID（如 `"1"`） |
| action.orders[].marketType | string | 是 | 固定 `"prediction"` |
| action.orders[].side | string | 是 | `"buy"` / `"sell"` |
| action.orders[].price | string | 是 | 挂单价格（如 `"0.65"`） |
| action.orders[].size | string | 是 | 下单数量（如 `"100"`） |
| action.orders[].clientOrderId | string | 是 | 下单clientOrderId 区分地区 |
| action.orders[].reduceOnly | boolean | 否 | 是否只减仓，默认 `false` |
| action.orders[].sizeType | string | 否 | `"base"`（默认）/ `"quote"` |
| action.orders[].orderType | object | 是 | 订单类型 |
| action.orders[].orderType.limit.tif | string | 是 | `"gtc"` / `"gtd"` / `"ioc"` / `"fok"` / `"alo"` |
| nonce | long | 是 | 请求时间戳（毫秒），防重放 |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**TIF（Time In Force）**

| JSON 值 | 交易结构 | 说明 |
| --- | --- | --- |
| `"gtc"` | "tif": "gtc" | Good-Til-Cancel，挂单直到成交或取消 |
| `"gtd"` | "tif": { "gtd": { "expiresAfter": 1700000005000 } }, | Good-Til-Date，挂单直到 expiresAfter 指定的时间过期或成交/取消 |
| `"ioc"` | "tif": "ioc" | Immediate-Or-Cancel（即 FAK），立即尽可能成交后取消剩余。**仅用于模拟市价单**，需配合价格保护上/下限（`price`） |
| `"fok"` | "tif": "fok" | Fill-Or-Kill，全部成交或全部取消。**仅用于模拟市价单**，需配合价格保护上/下限（`price`） |
| `"alo"` | "tif": "alo" | Add-Liquidity-Only (Post-Only)，只挂 maker 单，若会立即成交则拒绝 |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...789"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

### 3.2 撤销单个订单 [rest-api-3-orders-api-3-2-cancel-a-single-order]

撤销一个活跃订单。开发者自行构造撤单 calldata 并签名，服务端校验订单归属和状态后提交至 TradeZone。

#### HTTP请求 [rest-api-3-orders-api-3-2-cancel-a-single-order-http-request]

`POST /api/v5/predictions/orders/cancel`

#### 请求体 [rest-api-3-orders-api-3-2-cancel-a-single-order-request-body]

> 请求示例

```json
{
  "action": {
    "type": "cancel",
    "cancels": [{
      "assetId": "1",
      "marketType": "prediction",
      "oid": "12345",
      "clientOrderId":"0x"
    }]
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | 撤单动作 |
| action.type | string | 是 | 固定 `"cancel"` |
| action.cancels | array | 是 | 撤单列表（每次一笔） |
| action.cancels[].assetId | string | 是 | TradeZone 资产 ID（如 `"1"`） |
| action.cancels[].marketType | string | 是 | 固定 `"prediction"` |
| action.cancels[].oid | string | 否 | 订单 ID与客户端ID 两个中必传一个 |
| action.cancels[].clientOrderId | string | 否 | 订单 ID与客户端ID 两个中必传一个 |
| nonce | long | 是 | 请求时间戳（毫秒） |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

### 3.3 查询单个订单 [rest-api-3-orders-api-3-3-get-a-single-order]

查询指定订单的完整详情（活跃或历史订单均可）

#### HTTP请求 [rest-api-3-orders-api-3-3-get-a-single-order-http-request]

`GET /api/v5/predictions/orders/{orderId}`

#### 路径参数 [rest-api-3-orders-api-3-3-get-a-single-order-path-parameters]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| orderId | string | 是 | 订单 ID |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "id": "1",
    "oid": "1",
    "clientOrderId": "1",
    "marketId": "1",
    "assetId": "1",
    "side": "BUY",
    "orderType": "GTC",
    "sizeType": "BASE",
    "size": "100",
    "price": "0.65",
    "expiration": null,
    "txHash": "0xdef...789",
    "status": "ACTIVE",
    "filledSize": "40",
    "filledAmount": "26",
    "failReason": null,
    "cancelReason": null,
    "oddsType": "points",
    "createdAt": "1710000000000",
    "updatedAt": "1710000000000"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| id | string | 订单 ID |
| oid | string | 订单 oid |
| clientOrderId | string | 订单 clientOrderId |
| marketId | string | 市场 ID |
| tokenId | string | YES/NO 代币地址 |
| assetId | string | TradeZone 资产 ID |
| side | string | `BUY` / `SELL` |
| orderType | string | `GTC` / `GTD` / `FOK` / `IOC` / `POST_ONLY` |
| sizeType | string | `BASE` / `QUOTE` |
| size | string | 原始下单数量 |
| price | string | 挂单价格 |
| expiration | string | GTD 过期时间戳（毫秒）；非 GTD 为 null |
| txHash | string | 当前阶段的交易哈希 |
| status | string | 订单状态（见下表） |
| filledSize | string | 已成交代币数量 |
| filledAmount | string | 已成交 xp 金额 |
| failReason | string | 失败原因（仅 status = `FAILED` 时有值） |
| cancelReason | string | 取消原因（系统触发的取消） |
| oddsType | string | 盘口类型：`points`=积分盘 |
| createdAt | string | 订单创建时间戳（毫秒） |
| updatedAt | string | 最后更新时间戳（毫秒） |

**订单状态**

| Status | Description |
| --- | --- |
| PENDING_PLACE | Submitted, waiting for on-chain confirmation |
| ACTIVE | Active open order |
| PENDING_CANCEL | Cancellation submitted, waiting for on-chain confirmation |
| FILLED | Fully filled |
| PARTIALLY_FILLED | Partially filled then cancelled or expired |
| FAILED | On-chain transaction failed |
| CANCELLED | Cancelled by user or system |
| EXPIRED | GTD order expired |

### 3.4 查询用户订单列表 [rest-api-3-orders-api-3-4-get-user-orders]

查询当前认证用户的订单列表，支持过滤和分页。

#### HTTP请求 [rest-api-3-orders-api-3-4-get-user-orders-http-request]

`GET /api/v5/predictions/orders`

#### 请求参数（Query） [rest-api-3-orders-api-3-4-get-user-orders-request-parameters-query]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| marketId | string | 否 | 按市场 ID 过滤 |
| status | string | 否 | `open`（PENDING_PLACE / ACTIVE / PENDING_CANCEL）、`closed`（FILLED / PARTIALLY_FILLED / CANCELLED / EXPIRED / FAILED），不传默认`open` |
| cursor | string | 否 | 分页游标 |
| limit | int | 否 | 每页条数（默认 20，最大 50） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "id": "1",
        "oid": "1",
        "clientOrderId": "1",
        "marketId": "1",
        "assetId": "1",
        "side": "BUY",
        "orderType": "GTC",
        "sizeType": "BASE",
        "size": "100",
        "price": "0.65",
        "expiration": null,
        "txHash": "0xdef...789",
        "status": "ACTIVE",
        "filledSize": "40",
        "filledAmount": "26",
        "failReason": null,
        "cancelReason": null,
        "oddsType": "points",
        "createdAt": "1710000000000",
        "updatedAt": "1710000000000"
      }
    ],
    "nextCursor": "eyJpZCI6MTIzfQ",
    "hasNext": false
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| list[].id | string | 订单 ID |
| list[].marketId | string | 市场 ID |
| list[].oid | string | 订单 oid |
| list[].clientOrderId | string | 订单 clientOrderId |
| list[].tokenId | string | YES/NO 代币地址 |
| list[].assetId | string | TradeZone 资产 ID |
| list[].side | string | `BUY` / `SELL` |
| list[].orderType | string | `GTC` / `GTD` / `FOK` / `IOC` / `POST_ONLY` |
| list[].sizeType | string | `BASE` / `QUOTE` |
| list[].size | string | 原始下单数量 |
| list[].price | string | 挂单价格 |
| list[].expiration | string | GTD 过期时间戳（毫秒）；非 GTD 为 null |
| list[].txHash | string | 当前阶段的交易哈希 |
| list[].status | string | 订单状态：`PENDING_PLACE` / `ACTIVE` / `PENDING_CANCEL` / `FILLED` / `PARTIALLY_FILLED` / `FAILED` / `CANCELLED` / `EXPIRED` |
| list[].filledSize | string | 已成交代币数量 |
| list[].filledAmount | string | 已成交 pts金额 |
| list[].failReason | string | 失败原因（仅 status = `FAILED` 时有值） |
| list[].cancelReason | string | 取消原因（系统触发的取消） |
| list[].oddsType | string | 盘口类型：`points`=积分盘 |
| list[].createdAt | String | 订单创建时间戳（毫秒） |
| list[].updatedAt | string | 最后更新时间戳（毫秒） |
| nextCursor | string | 下一页游标 |
| hasNext | boolean | 是否有更多数据 |

### 3.5 撤销全部 / 指定市场订单 [rest-api-3-orders-api-3-5-cancel-all-market-orders]

撤销当前认证用户的活跃订单。assetIds 为空列表时撤销全部市场订单；传入具体值时仅撤销对应资产所在市场的订单。

#### HTTP请求 [rest-api-3-orders-api-3-5-cancel-all-market-orders-http-request]

`POST /api/v5/predictions/orders/cancel-all`

#### 请求体 [rest-api-3-orders-api-3-5-cancel-all-market-orders-request-body]

> 请求示例（按市场撤单）

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [1, 2, 5],
    "marketType": "prediction"
  },
  "nonce": 1708929600000,
  "expiresAfter": 1708929660000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

> 请求示例（撤销全部）

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [],
    "marketType": "prediction"
  },
  "nonce": 1708929600000,
  "expiresAfter": 1708929660000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | 撤单动作 |
| action.type | string | 是 | 固定 `"cancelAll"` |
| action.assetIds | array | 是 | 资产 ID 列表；传空列表撤销全部订单，传具体值按对应市场撤单 |
| action.marketType | string | 是 | 市场类型，固定 `"prediction"` |
| nonce | long | 是 | 请求时间戳（毫秒） |
| expiresAfter | long | 是 | 过期时间戳（毫秒） |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

### 3.6 发送心跳 [rest-api-3-orders-api-3-6-send-heartbeat]

心跳机制，用于保护开发者的活跃订单。请求体携带一个预签名的 `cancelAll` action，`nonce` 设置为当前时间 \+ 5 分钟。服务端暂存该签名；若开发者在 5 分钟内未续期心跳，系统将自动使用该签名执行全部撤单。

开发者需持续调用此接口（建议间隔 < 5 分钟），每次用新的 `nonce`（当前时间 \+ 5 分钟）覆盖上一次的预签名。

#### HTTP请求 [rest-api-3-orders-api-3-6-send-heartbeat-http-request]

`POST /api/v5/predictions/heartbeat`

#### 请求体 [rest-api-3-orders-api-3-6-send-heartbeat-request-body]

> 请求示例

```json
{
  "action": {
    "type": "cancelAll",
    "assetIds": [],
    "marketType": "prediction"
  },
  "nonce": 1708929900000,
  "expiresAfter": 1708929960000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | 撤单动作，结构与 `POST /api/v5/predictions/orders/cancel-all` 一致 |
| action.type | string | 是 | 固定 `"cancelAll"` |
| action.assetIds | array | 是 | 固定传空列表 `[]`，心跳触发时撤销全部订单 |
| action.marketType | string | 是 | 市场类型，固定 `"prediction"` |
| nonce | long | 是 | 当前时间 \+ 5 分钟的时间戳（毫秒） |
| expiresAfter | long | 是 | 过期时间戳（毫秒） |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "serverTimestamp": 1708929600000,
    "expireAt": 1708929900000
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| serverTimestamp | long | 服务端当前时间戳（毫秒） |
| expireAt | long | 本次心跳过期时间戳（毫秒） |

<aside class="notice">
<code>nonce</code> = 1708929900000 即当前时间 1708929600000 + 300000（5 分钟）。
</aside>

**订单语义说明**

<aside class="notice">
系统会限制自成交：同一钱包地址的买卖单不会互相成交。
</aside>

**订单模式支持矩阵**

**支持矩阵**

| 业务场景 | sizeType | tif | size | price 语义 | 支持 |
| --- | --- | --- | --- | --- | --- |
| 限价买入 | `base` | `gtc` / `gtd` / `ioc` / `fok` / `alo` | 用户输入 shares | 用户输入限价 | ✅ |
| 限价卖出 | `base` | `gtc` / `gtd` / `ioc` / `fok` / `alo` | 用户输入 shares | 用户输入限价 | ✅ |
| 市价买入（按数量） | `base` | `ioc` / `fok` | 用户输入 shares | 系统按盘口模拟生成的**最差成交价**（保护价） | ✅ |
| 市价买入（按金额） | `quote` | `ioc` | 用户输入名义金额（pts） | 系统按盘口模拟生成的**最差成交价**（保护价） | ✅ |
| 市价卖出（按数量） | `base` | `ioc` / `fok` | 用户输入 shares | 系统按盘口模拟生成的**最差成交价**（保护价） | ✅ |
| 市价买入（按金额）\+ FOK | `quote` | `fok` | — | — | ❌ **不支持** |

**关键规则**

1. **`ioc` 在预测市场中等价于业界常用的 `fak`**：能成交多少成交多少，剩余取消，不会留单。

2. **`fok` 仅支持按 shares 下单**：`sizeType=quote` 与 `tif=fok` 组合会被服务端拒绝。

3. **`alo`（Post Only）仅适用于限价单**：与 `sizeType=quote` 或市价单组合会被拒绝。

4. **市价单不是"无保护价单"**：在下单时根据盘口模拟出 `worstPrice`，并以此作为 `price` 提交至 TradeZone。实际成交价格**不会差于 `worstPrice`**。

5. **`tif=gtd` 必须传 `expiration`**：字段路径为 `action.orders[].orderType.limit.expiration`，类型为 `String`（Unix 毫秒时间戳，13 位），**绝对时间**。未传或非法时当前抛 `10001 PARAM_ERROR`。

**不支持组合的错误返回**

| 触发场景 | 当前错误码 |
| --- | --- |
| `sizeType=quote` 且 `tif=fok` | `10001` PARAM_ERROR |
| `sizeType=quote` 且 `tif ∈ {gtc, gtd, alo}` | `10001` PARAM_ERROR |
| `tif=gtd` 但未传 `expiration` | `10001` PARAM_ERROR |

**最小下单金额与清仓例外**

**最小下单金额规则**

- 预测市场对单笔名义金额有最小值约束，1pts

**清仓例外（Sell-All Exception）**

- 用户卖出某方向持仓后，该 assetId 剩余**总持仓**（= 可用 \+ 冻结）为 0，视为**清仓卖出**

- 清仓卖出场景下，即使名义成交金额低于 1 pts，也允许提交订单。

**价格单位与精度规则**

**合法范围**

| 条件 | 是否允许 |
| --- | --- |
| `price ≤ 0` | 拒绝 |
| `price ≥ 1` | 拒绝 |
| `0 < price < 1` | 允许，进入精度校验 |

**分段精度规则**

精度按 `[0.04, 0.96]` 与区间外分两段：

| 区间 | 最大小数位数 | 等价 cent 区间 | 示例（允许） | 示例（拒绝） |
| --- | --- | --- | --- | --- |
| `0 < price < 0.04` | **3 位** | `(0, 4)` cent | `0.001` / `0.025` / `0.039` | `0.0125`（4 位） |
| `0.04 ≤ price ≤ 0.96` | **2 位**（tick=0.01） | `[4, 96]` cent | `0.04` / `0.25` / `0.96` | `0.045` / `0.123` / `0.961` |
| `0.96 < price < 1` | **3 位** | `(96, 100)` cent | `0.962` / `0.9875` / `0.999` | `0.99875`（4 位） |

<aside class="notice">
<b>边界值：</b> <code>0.04</code> 与 <code>0.96</code> 都属于中间区间（<code>[0.04, 0.96]</code> 闭区间），适用 <b>2 位小数限制</b>。
</aside>

**ClientOrderId 生成规范**

`clientOrderId`（简称 cloid）是订单在链上的唯一标识。链上事件全球广播，各区域消费方通过 cloid 中的 region/env 前缀筛选出属于自己的订单。任何接入方生成的 cloid 都必须遵循本规范，否则订单会被误判归属。

**格式**

格式为 `0x{region}{env}{random}`：

| 段 | 长度（字符） | 内容 | 说明 |
| --- | --- | --- | --- |
| 前缀 | 2 | `0x` | 固定字面量 |
| region | 1 | 1 个 hex 字符 | 区域编码 |
| env | 1 | 1 个 hex 字符 | 环境编码 |
| random | 30 | 30 个 hex 字符 | 随机数（小写） |

总长固定 **34 字符**，全部为小写 hex（`0-9`, `a-f`）。

**region / env 编码表**

| region | 含义 |
| --- | --- |
| `0` | HK |
| `1` | US |
| `2` | EU |

| env | 含义 |
| --- | --- |
| `1` | 线上 |

取值都是 1 个 hex 字符，目前用到 0/1/2，后续扩展不超过 `f`（15）。

**random 生成要求**

- **熵**：≥ 120 bit（30 个 hex = 120 bit）
- **来源**：必须使用密码学安全或等效强度的随机源（如 UUIDv4、`crypto.randomBytes`、`secrets` 等）
- **编码**：小写 hex，不足位高位补 0

**参考实现**

> Java（UUID 派生）

```java
UUID u = UUID.randomUUID();
long hi = u.getMostSignificantBits();
long lo = u.getLeastSignificantBits();
StringBuilder sb = new StringBuilder("0x");
sb.append(Integer.toHexString(region & 0xF));
sb.append(Integer.toHexString(env & 0xF));
for (int i = 14; i >= 0; i--) {
    sb.append(Character.forDigit((int)((hi >>> (i * 4)) & 0xF), 16));
}
for (int i = 14; i >= 0; i--) {
    sb.append(Character.forDigit((int)((lo >>> (i * 4)) & 0xF), 16));
}
return sb.toString();
```

> Node.js

```javascript
const crypto = require('crypto');
function generate(region, env) {
  const rand = crypto.randomBytes(15).toString('hex'); // 30 hex
  return `0x${region.toString(16)}${env.toString(16)}${rand}`;
}
```

```python
import secrets
def generate(region: int, env: int) -> str:
    return f"0x{region:x}{env:x}{secrets.token_hex(15)}"
```

> Go

```go
import (
    "crypto/rand"
    "encoding/hex"
    "fmt"
)

func Generate(region, env int) (string, error) {
    b := make([]byte, 15)
    if _, err := rand.Read(b); err != nil {
        return "", err
    }
    return fmt.Sprintf("0x%x%x%s", region, env, hex.EncodeToString(b)), nil
}
```

**服务端解析与归属判断**

接收方按以下规则判断 cloid 是否属于当前环境：

```text
if cloid is null/empty
  or length < 4
  or 不以 "0x" 开头
  or cloid[2], cloid[3] 不是合法 hex 字符:
    视为 HK-预发（region=0, env=0）  ← 兜底规则
else:
    region = parse_hex(cloid[2])
    env    = parse_hex(cloid[3])
    判断是否等于当前环境的 (region, env)
```

**常见问题**

- 心跳检测： 如果运行机器人，设置心跳以在机器人断线时自动撤销所有订单。每 < 5 分钟发送一次预签名的全部撤单，详情见api文档

- Nonce： 使用当前毫秒时间戳。每个 Nonce 只能使用一次。

- 时间戳偏差： OK-ACCESS-TIMESTAMP 与服务器时间相差不能超过 30 秒，否则返回错误码 50102。

## 4. 仓位操作 [rest-api-4-positions-operations-api]

### 4.1 Split（xp → YES \+ NO） [rest-api-4-positions-operations-api-4-1-split-xp-yes-no]

将 xp 分割为等量的 YES 和 NO 条件代币对。开发者自行构造 calldata 并签名，服务端校验后直接提交至 TradeZone。

#### HTTP请求 [rest-api-4-positions-operations-api-4-1-split-xp-yes-no-http-request]

`POST /api/v5/predictions/positions/split`

#### 请求体 [rest-api-4-positions-operations-api-4-1-split-xp-yes-no-request-body]

> 请求示例

```json
{
  "action": {
    "type": "predictionSplit",
    "marketId": "1",
    "size": "100000000"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | Split 动作 |
| action.type | string | 是 | 固定 `"predictionSplit"` |
| action.marketId | string | 是 | 市场 ID（如 `"1"`） |
| action.size | string | 是 | pts 数量（最小单位字符串，如 `"100000000"`） |
| nonce | long | 是 | 请求时间戳（毫秒），防重放 |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

### 4.2 Merge（YES \+ NO → xp） [rest-api-4-positions-operations-api-4-2-merge-yes-no-xp]

将等量的 YES 和 NO 条件代币合并回 xp（Split 的逆操作）。开发者自行构造 calldata 并签名，服务端校验后直接提交至 TradeZone。

#### HTTP请求 [rest-api-4-positions-operations-api-4-2-merge-yes-no-xp-http-request]

`POST /api/v5/predictions/positions/merge`

#### 请求体 [rest-api-4-positions-operations-api-4-2-merge-yes-no-xp-request-body]

> 请求示例

```json
{
  "action": {
    "type": "predictionMerge",
    "marketId": "1",
    "size": "100000000"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | Merge 动作 |
| action.type | string | 是 | 固定 `"predictionMerge"` |
| action.marketId | string | 是 | 市场 ID（如 `"1"`） |
| action.size | string | 是 | 合并数量（最小单位字符串，如 `"100000000"`） |
| nonce | long | 是 | 请求时间戳（毫秒），防重放 |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

### 4.3 Redeem（结算后兑换 xp） [rest-api-4-positions-operations-api-4-3-redeem-redeem-xp-after-settlement]

市场结算后，用胜出的条件代币按 1:1 兑换 xp。兑换数量无需传入，默认兑换用户持有的全部胜出代币。

<aside class="notice">
市场结算后，系统会<b>自动</b>为所有持有胜出代币的用户兑换 xp（需等待一段时间）。<b>多数情况下无需手动调用本接口</b>；仅当自动兑换失败、或不想等待自动兑换期间，才需主动调用 Redeem 作为兜底。
</aside>

#### HTTP请求 [rest-api-4-positions-operations-api-4-3-redeem-redeem-xp-after-settlement-http-request]

`POST /api/v5/predictions/positions/redeem`

#### 请求体 [rest-api-4-positions-operations-api-4-3-redeem-redeem-xp-after-settlement-request-body]

> 请求示例

```json
{
  "action": {
    "type": "predictionRedeem",
    "marketId": "1"
  },
  "nonce": 1708929600000,
  "signature": {
    "Ecdsa": {
      "r": "0x...",
      "s": "0x...",
      "v": 1
    }
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| action | object | 是 | Redeem 动作 |
| action.type | string | 是 | 固定 `"predictionRedeem"` |
| action.marketId | string | 是 | 市场 ID（如 `"1"`） |
| nonce | long | 是 | 请求时间戳（毫秒），防重放 |
| signature | object | 是 | 签名对象 |
| signature.Ecdsa | object | 是 | ECDSA 签名分量 |
| signature.Ecdsa.r | string | 是 | 签名 r 值（`"0x..."`） |
| signature.Ecdsa.s | string | 是 | 签名 s 值（`"0x..."`） |
| signature.Ecdsa.v | int | 是 | 签名 v 值（`0` 或 `1`） |

<aside class="warning">
注意：与 Split/Merge 不同，Redeem 的 action 中<b>没有 <code>size</code> 字段</b>，默认兑换全部胜出代币。
</aside>

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "txHash": "0xdef...abc"
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| txHash | string | TradeZone 交易哈希 |

## 5. 成交历史 [rest-api-5-trade-history-api]

### 5.1 查询成交记录 [rest-api-5-trade-history-api-5-1-query-trades]

查询当前认证用户的成交（Fill）记录。服务端从登录态解析 userId 并内部转换为 address 查询。

#### HTTP请求 [rest-api-5-trade-history-api-5-1-query-trades-http-request]

`GET /api/v5/predictions/trades`

#### 请求参数（Query） [rest-api-5-trade-history-api-5-1-query-trades-request-parameters-query]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| marketId | string | 否 | 按市场 ID 过滤 |
| side | string | 否 | 按方向过滤：`BUY` / `SELL` |
| startTime | long | 否 | 起始时间戳（毫秒），包含 |
| endTime | long | 否 | 结束时间戳（毫秒），不包含 |
| cursor | string | 否 | 分页游标 |
| limit | int | 否 | 每页条数（默认 20，最大 100） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "tradeId": "1",
        "orderId": "1",
        "marketId": "1",
        "tokenId": "0xabc...001",
        "side": "BUY",
        "size": "50",
        "amount": "32.5",
        "price": "0.65",
        "fee": "0.065",
        "role": "TAKER",
        "txHash": "0xdef...789",
        "createdAt": 1710000030000
      }
    ],
    "nextCursor": "eyJpZCI6MTAwMDF9",
    "hasNext": false
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| list[].tradeId | string | 成交记录 ID |
| list[].orderId | string | 关联订单 ID |
| list[].marketId | string | 市场 ID |
| list[].tokenId | string | 代币地址（YES 或 NO） |
| list[].side | string | `BUY` / `SELL` |
| list[].size | string | 成交代币数量 |
| list[].amount | string | 成交 xp 金额 |
| list[].price | string | 成交价格 |
| list[].fee | string | 手续费（xp） |
| list[].role | string | `MAKER` / `TAKER` |
| list[].txHash | string | 链上交易哈希 |
| list[].createdAt | string | 成交时间戳（毫秒） |
| nextCursor | string | 下一页游标 |
| hasNext | boolean | 是否有更多数据 |

## 6. 仓位查询 [rest-api-6-positions-query-api]

### 6.1 查询当前持仓 [rest-api-6-positions-query-api-6-1-query-current-positions]

查询当前认证用户的活跃持仓。

#### HTTP请求 [rest-api-6-positions-query-api-6-1-query-current-positions-http-request]

`GET /api/v5/predictions/positions?status=open`

### 6.2 查询已平仓仓位 [rest-api-6-positions-query-api-6-2-query-closed-positions]

查询当前认证用户已退出或已结算的仓位。

#### HTTP请求 [rest-api-6-positions-query-api-6-2-query-closed-positions-http-request]

`GET /api/v5/predictions/positions?status=closed`

### 6.3 查询指定市场仓位 [rest-api-6-positions-query-api-6-3-query-positions-in-a-specified-market]

查询当前认证用户在指定市场的仓位。

#### HTTP请求 [rest-api-6-positions-query-api-6-3-query-positions-in-a-specified-market-http-request]

`GET /api/v5/predictions/positions?marketId={marketId}`

### 6.4 统一端点 [rest-api-6-positions-query-api-6-4-unified-endpoint]

以上三种场景均通过同一端点 \+ 查询参数组合实现：

#### HTTP请求 [rest-api-6-positions-query-api-6-4-unified-endpoint-http-request]

`GET /api/v5/predictions/positions`

#### 请求参数（Query） [rest-api-6-positions-query-api-6-4-unified-endpoint-request-parameters-query]

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| status | string | 否 | `open`（活跃持仓）、`closed`（已清仓），不传返回全部 |
| marketId | long | 否 | 按市场 ID 过滤 |
| cursor | string | 否 | 分页游标 |
| limit | int | 否 | 每页条数（默认 20，最大 100） |

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": {
    "list": [
      {
        "id": "100001",
        "tokenId": "0xabc...001",
        "marketId": "1",
        "tokenIndex": "1",
        "tokenName": "Yes",
        "size": "500",
        "availableSize": "480",
        "value": "325",
        "avgPrice": "0.62",
        "unRealizedPnl": "15",
        "unRealizedPnlPercentage": "0.048",
        "title": "Will BTC exceed $100k by end of 2026?",
        "icon": "https://cdn.example.com/markets/1.png",
        "eventId": "100",
        "winningToken": null,
        "positionStatus": 1,
        "oddsType": "points",
        "curPrice": "0.65",
        "realizedPnl": "12.5",
        "realizedPnlPercentage": "0.04"
      }
    ],
    "nextCursor": "eyJpZCI6MTAwMDAxfQ",
    "hasNext": false
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| list[].id | string | 仓位 ID |
| list[].tokenId | string | 代币 ID |
| list[].marketId | string | 市场 ID |
| list[].tokenIndex | string | 代币方向（`"1"` = YES，`"2"` = NO） |
| list[].tokenName | string | 代币名称（`"Yes"` / `"No"`） |
| list[].size | string | 持仓数量（= remain） |
| list[].availableSize | string | 可用持仓量（= remain − frozen，未被 SELL 挂单锁定的数量） |
| list[].value | string | 当前市值（curPrice × size） |
| list[].avgPrice | string | 加权平均持仓成本 |
| list[].unRealizedPnl | string | 未实现盈亏 |
| list[].unRealizedPnlPercentage | string | 未实现盈亏百分比 |
| list[].title | string | 市场问题文本 |
| list[].icon | string | 市场图标 URL |
| list[].eventId | string | 父事件 ID |
| list[].winningToken | string | 结算后获胜方代币 ID；未结算时为 null |
| list[].positionStatus | integer | 仓位状态码（见 PositionStatusEnum） |
| list[].oddsType | string | 盘口类型：`points`=积分盘 |
| list[].curPrice | string | 当前代币实时价格 |
| list[].realizedPnl | string | 已实现盈亏 |
| list[].realizedPnlPercentage | string | 已实现盈亏百分比 |
| nextCursor | string | 下一页游标 |
| hasNext | boolean | 是否有更多数据 |

## 7. 账户余额 [rest-api-7-account-balance-api]

### 7.1 查询账户余额 [rest-api-7-account-balance-api-7-1-query-account-balance]

查询当前认证用户的积分余额。

#### HTTP请求 [rest-api-7-account-balance-api-7-1-query-account-balance-http-request]

`GET /api/v5/predictions/balance`

**请求参数**：无

**响应**（`data`）：

> 响应示例

```json
{
  "code": 0,
  "message": "OK",
  "data": [
    { "oddsType": "points", "balance": "2.2",  "available": "2.2"  }
  ]
}
```

`data` 为数组，每个元素对应一种盘口类型的余额：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| oddsType | string | 盘口类型：`points`=积分盘 |
| balance | string | 总余额 |
| available | string | 可用余额（总余额 - 冻结中的金额） |

## 8. 限流（Rate Limits） [rest-api-8-rate-limits]

**事件与市场 API**

| 端点 | 限额 |
| --- | --- |
| `GET /api/v5/predictions/events` | 20 次 / 1s |
| `GET /api/v5/predictions/events/{eventId}` | 20 次 / 1s |
| `GET /api/v5/predictions/events/{eventId}/markets` | 20 次 / 1s |
| `GET /api/v5/predictions/markets/{marketId}` | 20 次 / 1s |
| `GET /api/v5/predictions/events/search` | 10 次 / 1s |

**价格 API（行情数据）**

| 端点 | 限额 |
| --- | --- |
| `GET /api/v5/market/ticker` | 10 次 / 1s |
| `GET /api/v5/market/candles` | 20 次 / 1s |
| `GET /api/v5/market/pm-books` | 20 次 / 1s |

**订单 API**

**查询类**

| 端点 | 限额 |
| --- | --- |
| `GET /api/v5/predictions/orders/{orderId}` | 20 次 / 1s |
| `GET /api/v5/predictions/orders` | 20 次 / 1s |

**写操作类**

| 端点 | 限额 |
| --- | --- |
| `POST /api/v5/predictions/orders` | 50 次 / 1s |
| `POST /api/v5/predictions/orders/cancel` | 20 次 / 1s |
| `POST /api/v5/predictions/orders/cancel-all` | 1 次 / 1h |
| `POST /api/v5/predictions/heartbeat` | 1 次 / 1s |

**仓位操作 API**

| 端点 | 限额 |
| --- | --- |
| `POST /api/v5/predictions/positions/split` | 5 次 / 1s |
| `POST /api/v5/predictions/positions/merge` | 5 次 / 1s |
| `POST /api/v5/predictions/positions/redeem` | 5 次 / 1s |

**成交历史 / 仓位查询 / 余额**

| 端点 | 限额 |
| --- | --- |
| `GET /api/v5/predictions/trades` | 10 次 / 1s |
| `GET /api/v5/predictions/positions` | 10 次 / 1s |
| `GET /api/v5/predictions/balance` | 10 次 / 1s |

**触发限流的响应**

- HTTP Status: `429 Too Many Requests`
- 业务错误码：`50011 RATE_LIMIT_EXCEEDED`
- 建议客户端按**指数退避**策略重试：首次 1s，后续每次翻倍，最大 30s；连续重试超过 5 次仍 429 时应停止并告警
