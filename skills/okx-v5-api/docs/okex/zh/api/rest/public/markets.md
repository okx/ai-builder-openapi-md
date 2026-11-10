---
site: okex
lang: zh
name: 获取市场
method: GET
path: /api/v5/public/event-contract/markets
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取市场 [public-data-rest-api-get-markets]

获取 OKX 预测市场某事件下的市场列表。返回数据按 expTime 和 floorStrike 降序排列。

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/event-contract/markets`

> 请求示例

```shell
GET /api/v5/public/event-contract/markets?seriesId=BTC-ABOVE-DAILY
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:------|:-----|:--------|:----|
| seriesId | String | 是 | 系列 ID，如 `BTC-ABOVE-DAILY` |
| eventId | String | 否 | 事件 ID，如 `BTC-ABOVE-DAILY-260224-1600` |
| instId | String | 否 | 产品 ID，如 `BTC-ABOVE-DAILY-260224-1600-65000` |
| state | String | 否 | 市场状态过滤。<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| limit | String | 否 | 返回结果数量，最大 100，默认 100 |
| before | String | 否 | 分页，返回早于请求 `expTime` 的更新记录，不包含该时间戳 |
| after | String | 否 | 分页，返回晚于请求 `expTime` 的更旧记录，不包含该时间戳 |

> 返回示例

```json
{
    "code": "0",
    "data": [
        {
            "seriesId": "BTC-ABOVE-DAILY",
            "eventId": "BTC-ABOVE-DAILY-260224-1600",
            "instId": "BTC-ABOVE-DAILY-260224-1600-65000",
            "listTime": "1769697132335",
            "expTime": "1769697132335",
            "state": "live",
            "fixTime": "",
            "outcome": "0",
            "floorStrike": "120000",
            "capStrike": "",
            "settleValue": "",
            "disputed": false,
            "hitDir": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
|:------|:-----|:----|
| seriesId | String | 系列 ID，如 `BTC-ABOVE-DAILY` |
| eventId | String | 事件 ID，如 `BTC-ABOVE-DAILY-260224-1600` |
| instId | String | 产品 ID，如 `BTC-ABOVE-DAILY-260224-1600-65000` |
| listTime | String | 上线时间。Unix时间戳的毫秒数格式，如 `1597026383085` |
| fixTime | String | 行权价格确定时间。Unix时间戳的毫秒数格式，如 `1597026383085`。仅适用于 `price_up_down` 结算方式。 |
| expTime | String | 该事件的行权时间。Unix时间戳的毫秒数格式，如 `1597026383085`。结算后更新。 |
| state | String | 市场状态。<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
| disputed | Boolean | 是否存在争议。<br>`true`<br>`false` |
| outcome | String | 市场结果。<br>`0`：未确定<br>`1`：YES<br>`2`：NO。<br>`1`/`2` 仅在 state 为 `expired` 时适用 |
| floorStrike | String | 导致 YES 结果的最低到期价格 |
| capStrike | String | `between` 结算方式中导致 YES 结果的最大到期值。`"INF"` 表示无上限（最高区间）。<br>非 `between` 方式返回 `""`。 |
| settleValue | String | 结算价格。<br>仅在 state 为 `expired` 时返回 |
| hitDir | String | 触及方向。仅在结算方式为 `hit` 时适用。<br>`up`：价格从下方触及<br>`dn`：价格从上方触及<br>`""`：不适用（非 `hit` 方式） |
