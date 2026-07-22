---
site: my
lang: zh
name: WebSocket
type: guide
---

# WebSocket [websocket]

## 1. WebSocket 登录认证 [websocket-1-websocket-login-authentication]

仅订阅私有频道前需先进行 WebSocket 登录认证。公共频道无需登录。

详见 [WebSocket 登录认证文档](https://www.okx.com/docs-v5/zh/#overview-websocket-login)。

> 通用订阅格式

```json
{
  "op": "subscribe",
  "args": [
     { "channel": "<频道名1>", "instId": "<yesAssetId1>" },
     { "channel": "<频道名2>", "instId": "<yesAssetId2>" }
  ]
}
```

> 通用取消订阅格式

```json
{
  "op": "unsubscribe",
  "args": [
      { "channel": "<频道名1>", "instId": "<yesAssetId1>" },
      { "channel": "<频道名2>", "instId": "<yesAssetId2>" }
   ]
}
```

## 2. WebSocket 私有频道 [websocket-2-websocket-private-channels]

| 频道名称 | 订阅参数 | 是否需要授权 | 说明 |
| --- | --- | --- | --- |
| pm-order | channelName | 是 | 用户订单数据推送 |
| pm-position | channelName | 是 | 仓位变更 |
| pm-user-trade | channelName | 是 | 用户交易历史推送 |
| pm-balance | channelName | 是 | 余额变更 |
| pm-pnl | channelName | 是 | 当前仓位的浮盈/浮亏数值 |

### 2.1 订单状态推送 [websocket-2-websocket-private-channels-2-1-order-status-push]

**推送频率：** 事件触发，订单状态变更时推送

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-order" }]
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-order", "uid": "{cexUserId}" },
  "data": [{
    "orderId": "307173036051017730",
    "clientOrderId": "cli-abc-123",
    "marketId": "100001",
    "status": "FILLED",
    "assetId": "71",
    "side": "BUY",
    "direction": "YES",
    "filledSize": "10",
    "orderSize": "10",
    "avgPrice": "0.57",
    "amount": "5.7",
    "limitPrice": "0.45",
    "failMessage": null,
    "oddsType": "points",
    "txHash": "0xdef...",
    "tradeId": "9876543210"
  }]
}
```

#### 推送字段说明 [websocket-2-websocket-private-channels-2-1-order-status-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | string | 订单 ID |
| `clientOrderId` | string / null | 客户端订单 ID（cloid）；客户端未传时为 null |
| `marketId` | string | 市场 ID |
| `status` | string | 推送事件类型（见下表枚举） |
| `assetId` | string | 币对资产 ID（yesAssetId 或 noAssetId） |
| `side` | string | 交易方向：`BUY` / `SELL` |
| `direction` | string | 持仓方向：`YES` / `NO` |
| `filledSize` | string / null | 本次累计成交份额；无成交时为 null |
| `orderSize` | string | 下单份额 |
| `avgPrice` | string / null | 累计成交均价（= amount / filledSize）；无成交时为 null |
| `amount` | string / null | 累计成交金额（xp），BUY=花费 / SELL=收入；无成交时为 null |
| `limitPrice` | string / null | 限价单挂单价格（仅限价场景）；市价单为 null |
| `failMessage` | string / null | 失败提示文案；仅 `PLACE_FAILED` / `CANCEL_FAILED` 场景填值 |
| `oddsType` | string | 盘口类型：`points` |
| `txHash` | string / null | 链上交易哈希；未上链事件为 null（如 `PLACE_FAILED`） |
| `tradeId` | string / null | TradeZone 成交 ID；仅限价单部分成交（status=`ACTIVE`）时填值 |

#### status 枚举 [websocket-2-websocket-private-channels-2-1-order-status-push-status-enum]

| code | 说明 | 必填字段 |
| --- | --- | --- |
| `ACTIVE` | 活跃（限价单上链 / 部分成交且剩余有效） | `orderSize`, `limitPrice`（部分成交时另含 `filledSize`, `avgPrice`, `amount`, `tradeId`） |
| `FILLED` | 完全成交 | `filledSize`, `avgPrice`, `amount`, `txHash` |
| `PARTIALLY_FILLED` | 部分成交后撤单 | `filledSize`, `orderSize`, `avgPrice`, `amount`, `txHash` |
| `PLACE_FAILED` | 下单失败 | `orderSize`, `failMessage` |
| `CANCEL_FAILED` | 撤单失败（非预期） | `orderId`, `failMessage` |
| `CANCELLED` | 用户主动撤单 / 系统批量撤 | `orderSize`, `limitPrice` |
| `EXPIRED` | 订单到期 | `orderSize`, `limitPrice` |

### 2.2 仓位变更推送 [websocket-2-websocket-private-channels-2-2-position-change-push]

**推送频率：** 事件触发

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-position" }]
}
```

### 2.3 仓位 [websocket-2-websocket-private-channels-2-3-position]

（status = FILL / REDEEM/FILL_FAILED/REDEEM_FAILED）

> 推送数据格式

```json
{
  "arg": { "channel": "pm-position", "uid": "{cexUserId}" },
  "data": [{
    "id": "307173036051017730",
    "marketId": "100001",
    "tokenId": "20000",
    "assetId": "71",
    "amount": "10",
    "timestamp": "1712736000000",
    "unRealizedPnl": "5.20",
    "unRealizedPnlPercentage": "1.05",
    "value": "100.00",
    "avgPrice": "0.57",
    "status": "FILL",
    "tradeId": "9876543210",
    "oddsType": "points"
  }]
}
```

#### 字段说明 [websocket-2-websocket-private-channels-2-3-position-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `id` | String | 仓位 ID |
| `marketId` | String | 市场 ID |
| `tokenId` | String | YES/NO token 链上 ID |
| `assetId` | String | 币对资产 ID（yesAssetId 或 noAssetId） |
| `amount` | String | 当前持仓数量（`remain` 快照），REDEEM 场景为 `"0"` |
| `timestamp` | String | 事件时间戳（毫秒） |
| `unRealizedPnl` | String | 未实现盈亏 = (currentPrice − avgCost) × remain；REDEEM 为 `"0"` |
| `unRealizedPnlPercentage` | String | 未实现盈亏百分比（8 位精度）；REDEEM 为 `"0"` |
| `value` | String | 仓位市值 = currentPrice × remain；REDEEM 为 `"0"` |
| `avgPrice` | String | 加权平均持仓成本；REDEEM 为 `"0"` |
| `status` | String | `FILL` / `REDEEM` |
| `tradeId` | String / null | TradeZone 成交 ID；仅 `FILL` 场景填值（`REDEEM` 为 null） |
| `oddsType` | String | 盘口类型：`points` |

### 2.4 status 适用场景 [websocket-2-websocket-private-channels-2-4-status-scenarios]

| code | 说明 | 单次推送条数 |
| --- | --- | --- |
| `FILL` | 订单成交导致仓位变更 | 1（单 tokenId 仓位） |
| `REDEEM` | 结算赎回，该市场仓位清零 | N（该市场原有的每个 tokenId 各一条，`amount=0`） |

### 2.5 仓位 [websocket-2-websocket-private-channels-2-5-position]

（status = SPLIT / MERGE / DEPOSIT / WITHDRAW/SPLIT_FAILED/MERGE_FAILED/DEPOSIT_FAILED/WITHDRAW_FAILED）

> 推送数据格式

```json
{
  "arg": { "channel": "pm-position", "uid": "{cexUserId}" },
  "data": [{
    "marketId": "100001",
    "status": "DEPOSIT",
    "amount": "100",
    "txHash": "0xdef...",
    "oddsType": "points",
    "ext": {
      "toTxHash": "0xabc..."
    }
  }]
}
```

#### 字段说明 [websocket-2-websocket-private-channels-2-5-position-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `marketId` | String | 市场 ID |
| `status` | String | `SPLIT` / `MERGE` / `DEPOSIT` / `WITHDRAW` |
| `amount` | String | 金额 |
| `txHash` | String | 链上交易哈希。DEPOSIT 场景填 XLayer 原始交易哈希；其余场景填该业务对应链上哈希 |
| `oddsType` | String | 盘口类型：`points` |
| `ext` | Object / null | 扩展信息；仅 `DEPOSIT` 场景填值，其余场景为 null |
| `ext.toTxHash` | String / null | TZ 侧入账交易哈希；仅 `DEPOSIT` 场景填值 |

#### status 枚举 [websocket-2-websocket-private-channels-2-5-position-status-enum]

| code | 说明 |
| --- | --- |
| `FILL` | 订单成交导致仓位变更 |
| `SPLIT` | Split 拆分成功 |
| `MERGE` | Merge 合并成功 |
| `REDEEM` | 结算赎回成功 |
| `DEPOSIT` | 充值成功 |
| `WITHDRAW` | 提现成功 |
| `FILL_FAILED` | 成交失败 |
| `SPLIT_FAILED` | Split 失败 |
| `MERGE_FAILED` | Merge 失败 |
| `REDEEM_FAILED` | 赎回失败 |
| `DEPOSIT_FAILED` | 充值失败 |
| `WITHDRAW_FAILED` | 提现失败 |

### 2.6 撮合成交推送 [websocket-2-websocket-private-channels-2-6-trade-fill-push]

每笔 FILL 事件推送一条成交流水，前端用于实时展示成交记录列表。

**推送频率：** 事件触发，每笔成交一条

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-user-trade" }]
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-user-trade", "uid": "{cexUserId}" },
  "data": [{
    "orderId": "307173036051017730",
    "clientOrderId": "cli-abc-123",
    "marketId": "100001",
    "tokenId": "20000",
    "assetId": "71",
    "side": "BUY",
    "size": "10",
    "price": "0.57",
    "txhash": "0xdef...",
    "timestamp": "1712736000000",
    "tradeId": "9876543210"
  }]
}
```

#### 推送字段说明 [websocket-2-websocket-private-channels-2-6-trade-fill-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `orderId` | string | TradeZone 订单 ID |
| `clientOrderId` | string / null | 客户端订单 ID；客户端未传时为 null |
| `marketId` | string | 市场 ID |
| `tokenId` | string | YES/NO token 链上 ID |
| `assetId` | string | 币对资产 ID（yesAssetId 或 noAssetId） |
| `side` | string | 成交方向：`BUY` / `SELL` |
| `size` | string | 本次成交数量 |
| `price` | string | 本次成交价格 |
| `txhash` | string | 链上交易哈希 |
| `timestamp` | string | 事件时间戳（毫秒） |
| `tradeId` | string | TradeZone 成交 ID |

### 2.7 余额变更推送 [websocket-2-websocket-private-channels-2-7-balance-change-push]

余额同步成功后推送，前端用于实时更新余额显示。

**推送频率：** 事件触发，余额变更（挂单 / 撤单 / 成交 / Split / Merge / 赎回 / 充值 / 提现）时推送

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-balance" }]
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-balance", "uid": "{cexUserId}" },
  "data": [{
    "walletAddress": "0x1234abcd5678ef901234abcd5678ef901234abcd",
    "available": "950.5",
    "total": "1000",
    "frozen": "49.5",
    "tokenId": "0",
    "changeType": "FILL",
    "changeAmount": "100",
    "updateTime": "1712736000000",
    "oddsType": "points"
  }]
}
```

#### 推送字段说明 [websocket-2-websocket-private-channels-2-7-balance-change-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `walletAddress` | String | 用户 AA 钱包地址 |
| `available` | String | 可用余额 |
| `total` | String | 总余额（含冻结） |
| `frozen` | String | 冻结金额（= total − available） |
| `tokenId` | String | xp token 链上 ID（与 `oddsType` 对应） |
| `changeType` | String | 触发原因（见下表枚举） |
| `changeAmount` | String / null | 本次变动金额；不适用场景为 null |
| `updateTime` | String | 事件时间戳（毫秒） |
| `oddsType` | String | 盘口类型：`points` |

### 2.8 changeType 枚举 [websocket-2-websocket-private-channels-2-8-changetype-enum]

| code | 触发场景 |
| --- | --- |
| `PLACE` | 挂单冻结 |
| `CANCEL` | 撤单解冻 |
| `FILL` | 成交导致余额变更 |
| `SPLIT` | Split 扣减 xp |
| `MERGE` | Merge 增加 xp |
| `REDEEM` | 结算赎回 |
| `DEPOSIT` | 充值 |
| `WITHDRAW` | 提现 |

### 2.9 Pnl变更推送 [websocket-2-websocket-private-channels-2-9-pnl-change-push]

同时推送Pnl曲线图及概览

> 推送数据格式

```json
{
  "arg": { "channel": "pm-pnl", "uid": "{cexUserId}" },
  "data": [{
    "portfolioValue": "1234.56",
    "periods": [
      { "period": "1D", "periodPnl": "12.34",  "pnlPercent": "1.01" },
      { "period": "1W", "periodPnl": "56.78",  "pnlPercent": "4.83" },
      { "period": "1M", "periodPnl": "120.45", "pnlPercent": "10.81" },
      { "period": "6M", "periodPnl": "320.10", "pnlPercent": "35.02" },
      { "period": "1Y", "periodPnl": "500.00", "pnlPercent": "67.93" }
    ]
  }]
}
```

#### 字段说明 [websocket-2-websocket-private-channels-2-9-pnl-change-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `portfolioValue` | String | 当前组合总价值（xp 可用余额 \+ 仓位市值） |
| `periods` | Array | 多周期 PnL 汇总数组 |
| `periods[].period` | String | 周期：`1D` / `1W` / `1M` / `6M` / `1Y` |
| `periods[].periodPnl` | String | 该周期内 PnL 绝对值 |
| `periods[].pnlPercent` | String | 该周期内 PnL 百分比 |

> 推送数据格式

```json
{
  "arg": { "channel": "pm-pnl", "uid": "{cexUserId}" },
  "data": [{
    "period": "0",
    "interval": "600000",
    "points": [
      { "time": "1712707200000", "pnl": "1000.00" },
      { "time": "1712728800000", "pnl": "1010.50" },
      { "time": "1712750400000", "pnl": "1023.75" }
    ],
    "currentPnl": "1023.75",
    "high": "1030.00",
    "low": "995.00"
  }]
}
```

#### 字段说明 [websocket-2-websocket-private-channels-2-9-pnl-change-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `period` | String | 周期编码：`0`=1D / `1`=1W / `2`=1M / `3`=6M / `4`=1Y |
| `interval` | String | 数据点间隔（毫秒），与周期对应（见下表） |
| `points` | Array | 时间序列数据点 |
| `points[].time` | String | 数据点时间戳（毫秒） |
| `points[].pnl` | String | 该时刻总资产（= xp 余额 \+ 仓位市值） |
| `currentPnl` | String | 当前总资产（兼容字段） |
| `high` | String | 周期内最高总资产 |
| `low` | String | 周期内最低总资产 |

### 2.10 周期编码与 interval 对照 [websocket-2-websocket-private-channels-2-10-period-code-and-interval-mapping]

| period 编码 | 周期 | interval（毫秒） | 含义 |
| --- | --- | --- | --- |
| `0` | 1D | `600000` | 10 分钟 |
| `1` | 1W | `1800000` | 30 分钟 |
| `2` | 1M | `3600000` | 1 小时 |
| `3` | 6M | `86400000` | 1 天 |
| `4` | 1Y | `86400000` | 1 天 |

## 3. WebSocket 公共频道 [websocket-3-websocket-public-channels]

### 3.1 深度推送 [websocket-3-websocket-public-channels-3-1-order-book-push]

### 3.2 推送频率 [websocket-3-websocket-public-channels-3-2-push-frequency]

- 最小推送间隔 100ms — 两次增量推送之间最短间隔

- 最大推送间隔 60,000ms (60s) — 订单簿无变化时最长推送间隔

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-books",
    "instId": "{yesAssetId}"
  }]
}
```

> 订阅成功响应

```json
{
  "event": "subscribe",
  "arg": { "channel": "pm-books", "instId": "{yesAssetId}" }
}
```

> 推送数据格式

```json
{                    
      "arg": {"channel": "pm-books", "instId": "{yesAssetId}"},
      "action": "snapshot",                   
      "data": [                               
          {                                                                                                                                                                                                                                                                                                                 
              "asks": [                                                                                                                                                                                                                                                                                                     
                  ["67300.1", "201.18", "25"],                                                                                                                                                                                                                                                                         
                  ["67300.2", "421.45", "5"]                                                                                                                                                                                                                                                                           
              ],                                                                                                                                                                                                                                                                                                            
              "bids": [                                                                                                                                                                                                                                                                                                     
                  ["67300", "525.41", "34"],                                                                                                                                                                                                                                                                           
                  ["67299.9", "0.17", "7"]                                                                                                                                                                                                                                                                             
              ],                                                                                                                                                                                                                                                                                                            
              "ts": "1774944028506",                                                                                                                                                                                                                                                                                        
              "checksum": -702280706,                                                                                                                                                                                                                                                                                       
              "seqId": 308306650401,                                                                                                                                                                                                                                                                                        
              "prevSeqId": -1                                                                                                                                                                                                                                                                                               
          }                                                                                                                                                                                                                                                                                                                 
      ]                                                                                                                                                                                                                                                                                                                     
  }  
```

### 3.3 成交推送 [websocket-3-websocket-public-channels-3-3-trade-push]

实时推送每笔成交记录

**推送频率**：推送间隔 200ms，每次推送最多 20 条成交，编码类型为聚合成交

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-trades",
    "instId": "{yesAssetId}"
  }]
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-trades", "instId": "{yesAssetId}" },
  "data": [{
    "instId": "{yesAssetId}",
    "tradeId": "123456789",
    "px": "0.65",
    "sz": "100",
    "side": "buy",
    "ts": "1711900800123"
  }]
}
```

#### 推送字段说明 [websocket-3-websocket-public-channels-3-3-trade-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `instId` | String | yesAssetId |
| `fId` | String | 聚合周期内首笔成交 ID |
| `lId` | String | 聚合周期内末笔成交 ID |
| `px` | String | 成交价格 |
| `sz` | String | 成交数量 |
| `side` | String | 成交方向：`buy` 买入，`sell` 卖出 |
| `ts` | String | 成交时间（Unix 毫秒时间戳） |

### 3.4 K 线推送 [websocket-3-websocket-public-channels-3-4-candlestick-push]

实时推送 K 线数据，当前 K 线未收盘时会持续更新推送。端上可基于K线的收盘价绘制价格趋势图，具体订阅的颗粒度由端上自行评估

**频道命名规则：** `pm-candle` \+ 周期，例如 `pm-candle1m`、`pm-candle15m`

> 订阅示例（以 1 分钟和 5 分钟为例）

```json
{
  "op": "subscribe",
  "args": [
    { "channel": "pm-candle1m", "instId": "{yesAssetId}" },
    { "channel": "pm-candle15m", "instId": "{yesAssetId}" }
  ]
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-candle15m", "instId": "{yesAssetId}" },
  "data": [
    ["1711900800000", "0.65", "0.67", "0.63", "0.66", "2000", "1300", "1300", "0"]
  ]
}
```

### 3.5 推送字段说明 [websocket-3-websocket-public-channels-3-5-push-field-descriptions]

（data 数组中每条记录按索引顺序）

| 索引 | 字段 | 说明 |
| --- | --- | --- |
| 0 | `ts` | K 线开始时间（Unix 毫秒时间戳） |
| 1 | `o` | 开盘价 |
| 2 | `h` | 最高价 |
| 3 | `l` | 最低价 |
| 4 | `c` | 收盘价 |
| 5 | `vol` | 成交量（以张计） |
| 6 | `volCcy` | 成交量（以计价货币计） |
| 7 | `volCcyQuote` | 成交量（以报价货币计） |
| 8 | `confirm` | K 线状态：`0` 未确认（当前 K 线），`1` 已确认（已收盘） |

### 3.6 行情 Ticker 推送 [websocket-3-websocket-public-channels-3-6-ticker-push]

实时推送最新成交价、买一价、卖一价和24小时交易量等信息。

**推送频率**：最快100ms推送一次，触发推送的事件有：成交、买一卖一发生变动，没有触发事件时不推送

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{ "channel": "pm-tickers", "instId": "{yesAssetId}" }]
}
```

> 订阅成功响应

```json
{
  "event": "subscribe",
  "arg": { "channel": "pm-tickers", "instId": "{yesAssetId}" },
  "connId": "accb8e21"
}
```

> 推送数据格式

```json
{
  "arg": { "channel": "pm-tickers", "instId": "{yesAssetId}" },
  "data": [{
    "instType": "PREDICTIONS",
    "instId": "{yesAssetId}",
    "last": "0.4999",
    "lastSz": "1",
    "askPx": "0.6",
    "askSz": "30",
    "bidPx": "0.4",
    "bidSz": "30",
    "open24h": "0.5001",
    "high24h": "0.5001",
    "low24h": "0.4999",
    "vol24h": "372",
    "volCcy24h": "186",
    "sodUtc0": "0.4999",
    "sodUtc8": "0.4999",
    "ts": "1774948808119"
  }]
}
```

#### 推送字段说明 [websocket-3-websocket-public-channels-3-6-ticker-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| instType | String | 产品类型 |
| instId | String | yesAssetId |
| last | String | 最新成交价 |
| lastSz | String | 最新成交数量 |
| askPx | String | 卖一价 |
| askSz | String | 卖一价对应数量 |
| bidPx | String | 买一价 |
| bidSz | String | 买一价对应数量 |
| open24h | String | 24小时开盘价 |
| high24h | String | 24小时最高价 |
| low24h | String | 24小时最低价 |
| vol24h | String | 24小时成交量（以张计） |
| volCcy24h | String | 24小时成交量（以计价货币计） |
| sodUtc0 | String | UTC 0 时开盘价 |
| sodUtc8 | String | UTC\+8 时开盘价 |
| ts | String | 数据推送时间（Unix 毫秒时间戳） |

### 3.7 概率价格推送 [websocket-3-websocket-public-channels-3-7-probability-price-push]

预测市场概率价格推送，包含市场概率、累计成交额、买一卖一、最新成交价等信息

**推送频率：**定时3s推送一次

> 订阅示例

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "prediction-market-prices",
    "instId": "{yesAssetId}"
  }]
}
```

> 推送数据格式

```json
{
    "arg": {
        "channel": "prediction-market-prices",
        "instId": "{yesAssetId}"
    },
    "data": [
        {
            "yesAssetId": "71",
            "eventId": "1774875348987717503",
            "bestBid": "0.4896",
            "bestAsk": "0.6134",
            "lastTradePrice": "0.5848",
            "probability": "5515",
            "marketVolume": "17.1463",
            "eventVolume": "773.6524",
            "timestamp": "1775036647300"
        }
    ]
}
```

#### 推送字段说明 [websocket-3-websocket-public-channels-3-7-probability-price-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| yesAssetId | String | 预测市场 yesAssetId |
| eventId | String | 市场所属的event |
| bestBid | String | 买一价（最优买入价） |
| bestAsk | String | 卖一价（最优卖出价） |
| lastTradePrice | String | 最新成交价 |
| probability | String | Yes 方向的市场概率，推送万分比整数（如 0.6500 → 6500） |
| marketVolume | String | 当前市场累计成交额 |
| eventVolume | String | 当前事件累计成交额 |
| timestamp | String | 事件时间戳（毫秒） |

### 3.8 事件状态推送 [websocket-3-websocket-public-channels-3-8-event-status-push]

事件状态变更，用于展示事件最终结算结果

**推送频率：**事件触发，事件得到最终结果时推送，世界杯期间一天2~3场比赛；多元互斥、单一二元事件拿到最终结果时进行推送

> 订阅示例

> **注意：** 订阅此频道时，`instId` 取值为 event`-{eventId}`

```json
{
  "op": "subscribe",
  "args": [{
    "channel": "pm-event-status",
    "instId": "event-{eventId}"
  }]
}
```

> 推送数据格式

```json
{
  "arg": {
    "channel": "pm-event-status",
    "instId": "event-{eventId}"
  },
  "data": [{
    "eventId": "{eventId}",
    "status": "resolved",
    "marketId":"marketId",
    "outcomeOption":"yes | no | others | 球队名称 | draw",
    "timestamp": "1672290687"
  }]
}
```

#### 推送字段说明 [websocket-3-websocket-public-channels-3-8-event-status-push-push-field-descriptions]

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| eventId | string | 比赛ID |
| status | string | 事件状态 |
| marketId | string | 胜出市场的marketId |
| outcomeOption | string | 最终胜出的展示 |
| timestamp | string | 事件时间戳（毫秒） |
