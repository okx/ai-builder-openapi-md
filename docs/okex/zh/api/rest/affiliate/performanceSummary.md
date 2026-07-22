---
site: okex
lang: zh
name: 获取节点业绩概览
method: GET
path: /api/v5/affiliate/performance/summary
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取节点业绩概览 [affiliate-rest-api-get-performance-summary]

获取指定时间窗口内节点的业绩聚合指标。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/performance/summary`

> 请求示例

```shell
GET /api/v5/affiliate/performance/summary?periodType=total
```

#### 请求参数

| **参数名**  | **类型**  | **是否必须** | **描述** |
| :--------- | :------- | :---------- | :------- |
| periodType | String   | 否          | 统计窗口（仅 `uTime` 不受影响）。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>`custom`：自定义窗口，需配合 `begin` 与 `end` 使用。<br>默认为 `total` |
| begin      | String   | 条件必填     | 自定义统计窗口起始时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `end` 同时传入。包含端点。 |
| end        | String   | 条件必填     | 自定义统计窗口结束时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `begin` 同时传入。包含端点。 |

当 `periodType=custom` 时，需同时传 `begin` 和 `end`，仅传一个会返回 `50014`。<br>
其他 `periodType` 值使用服务端预设窗口，与之同时传入的 `begin` / `end` 将被忽略。<br>
`periodType` / `begin` / `end` 仅作用于 `inviteeCnt`、`depAmt`、`details.*` 等汇总字段，`uTime` 始终返回最新数据快照时间，与窗口无关。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "uTime": "1777541513000",
            "inviteeCnt": "102",
            "depAmt": "1756.287846940199989393",
            "details": [
                {
                    "commissionCategory": "SPOT",
                    "firstTraderCnt": "17",
                    "traderCnt": "17",
                    "vol": "21548.6417826825604",
                    "commission": "3.322319946747010328"
                },
                {
                    "commissionCategory": "DERIVATIVE",
                    "firstTraderCnt": "9",
                    "traderCnt": "9",
                    "vol": "94531.94802",
                    "commission": "3.25142568535855"
                },
                {
                    "commissionCategory": "BSC",
                    "firstTraderCnt": "0",
                    "traderCnt": "0",
                    "vol": "0",
                    "commission": "0"
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名**            | **类型**          | **描述** |
| :------------------- | :--------------- | :------- |
| uTime                | String           | 数据最近更新时间，Unix时间戳的毫秒数格式。 |
| inviteeCnt           | String           | 直客总数。 |
| depAmt               | String           | 累计充值金额，单位为 `USDT`。 |
| details              | Array of objects | 按业务类别拆分的明细，每个类别一条。 |
| > commissionCategory | String           | 返佣计算类别。<br>`SPOT`：现货<br>`DERIVATIVE`：衍生品<br>`BSC`：返佣业务 |
| > firstTraderCnt     | String           | 在选定窗口内首次交易的直客数（按 `commissionCategory` 维度）。每个直客在生命周期内最多统计一次。 |
| > traderCnt          | String           | 选定窗口内在该 `commissionCategory` 下产生交易的直客数。按窗口统计。 |
| > vol                | String           | 选定窗口内该 `commissionCategory` 下的交易量，单位为 `USDT`。按窗口统计——区别于 `/invitee/list`、`/sub-affiliate/list` 中的 `totalVol`（生命周期累计）。 |
| > commission         | String           | 选定窗口内该 `commissionCategory` 下的返佣，单位为 `USDT`。按窗口统计——区别于其他接口中的 `totalCommission`（生命周期累计）。 |
