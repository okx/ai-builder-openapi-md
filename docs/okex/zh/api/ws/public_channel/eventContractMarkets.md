---
site: okex
lang: zh
name: 事件合约市场频道
type: guide
---

### 事件合约市场频道 [public-data-websocket-event-contract-markets-channel]

推送事件合约市场状态更新及 floorStrike 生成。不推送初始快照。

#### URL Path

/ws/v5/public

> 请求示例

```shell
{
    "op": "subscribe",
    "args": [
        {
            "channel": "event-contract-markets",
            "instType": "EVENTS"
        }
    ]
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:------|:-----|:--------|:----|
| id | String | 否 | 消息的唯一标识。用户提供，返回参数中会返回以便于找到相应的请求。字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op | String | 是 | 操作。<br>`subscribe`<br>`unsubscribe` |
| args | Array of objects | 是 | 订阅频道列表 |
| > channel | String | 是 | 频道名。<br>`event-contract-markets` |
| > instType | String | 是 | 产品类型。<br>`EVENTS` |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "event-contract-markets",
        "instType": "EVENTS"
    },
    "connId": "a4d3ae55"
}
```

> 失败返回示例

```json
{
    "id": "1512",
    "event": "error",
    "code": "60012",
    "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{\"channel\": \"event-contract-markets\", \"instType\": \"EVENTS\"}]}",
    "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
|:------|:-----|:----|
| id | String | 消息的唯一标识 |
| event | String | 事件。<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg | Object | 订阅的频道 |
| > channel | String | 频道名 |
| > instType | String | 产品类型 |
| code | String | 错误码 |
| msg | String | 错误消息 |
| connId | String | WebSocket 连接 ID |

> 推送数据示例

```json
{
    "arg": {
        "channel": "event-contract-markets"
    },
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "eventId": "BTC-ABOVE-DAILY-260224-1600",
            "instId": "BTC-ABOVE-DAILY-260224-1600-65000",
            "listTime": "1769697132335",
            "fixTime": "",
            "expTime": "1769697132335",
            "state": "live",
            "outcome": "0",
            "floorStrike": "120000",
            "capStrike": "",
            "settleValue": "",
            "disputed": false,
            "hitDir": ""
        }
    ]
}
```

#### 推送数据参数

| 参数名 | 类型 | 描述 |
|:------|:-----|:----|
| arg | Object | 订阅的频道 |
| > channel | String | 频道名 |
| data | Array of objects | 订阅数据 |
| > seriesId | String | 系列 ID，如 `BTC-ABOVE-DAILY` |
| > eventId | String | 事件 ID，如 `BTC-ABOVE-DAILY-260224-1600` |
| > instId | String | 产品 ID，如 `BTC-ABOVE-DAILY-260224-1600-65000` |
| > listTime | String | 上线时间。Unix时间戳的毫秒数格式，如 `1597026383085` |
| > fixTime | String | 行权价格确定时间。Unix时间戳的毫秒数格式，如 `1597026383085`。仅适用于 `price_up_down` 结算方式。 |
| > expTime | String | 行权时间。Unix时间戳的毫秒数格式，如 `1597026383085`。结算后更新。 |
| > state | String | 市场状态。<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| > outcome | String | 市场结果。<br>`0`：未确定<br>`1`：YES<br>`2`：NO。<br>`1`/`2` 仅在 state 为 `expired` 时适用 |
| > floorStrike | String | 导致 YES 结果的最低到期价格 |
| > capStrike | String | `between` 结算方式中导致 YES 结果的最大到期值。`"INF"` 表示无上限（最高区间）。<br>非 `between` 方式返回 `""`。 |
| > settleValue | String | 结算价格。<br>仅在 state 为 `expired` 时返回 |
| > disputed | Boolean | 是否存在争议。<br>`true`<br>`false` |
| > hitDir | String | 触及方向。仅在结算方式为 `hit` 时适用。<br>`up`：价格从下方触及<br>`dn`：价格从上方触及<br>`""`：不适用（非 `hit` 方式） |
