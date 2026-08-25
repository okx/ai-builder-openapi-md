---
site: okex
lang: zh
name: 获取事件
method: GET
path: /api/v5/public/event-contract/events
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取事件 [public-data-rest-api-get-events]

获取 OKX 预测市场某系列下的事件列表，包含已到期事件。返回数据按 expTime 和 eventId 降序排列。

#### 限速：10次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/event-contract/events`

> 请求示例

```shell
GET /api/v5/public/event-contract/events?seriesId=BTC-ABOVE-DAILY
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:------|:-----|:--------|:----|
| seriesId | String | 是 | 系列 ID，如 `BTC-ABOVE-DAILY` |
| eventId | String | 否 | 事件 ID，如 `BTC-ABOVE-DAILY-260224-1600` |
| state | String | 否 | 事件状态过滤。<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
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
            "expTime": "1769697132335",
            "state": "live"
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
| fixTime | String | 执行价格确定时间。Unix时间戳的毫秒数格式，如 `1597026383085`。仅适用于 `price_up_down` 结算方式。 |
| expTime | String | 该事件的行权时间。Unix时间戳的毫秒数格式，如 `1597026383085` |
| state | String | 事件状态。<br>`preopen`<br>`live`<br>`settling`<br>`expired` |
