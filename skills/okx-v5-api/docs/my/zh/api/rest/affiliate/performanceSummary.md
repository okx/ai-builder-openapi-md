---
site: my
lang: zh
name: 获取 CPS 节点业绩概览
method: GET
path: /api/v5/affiliate/performance/summary
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取 CPS 节点业绩概览 [affiliate-rest-api-get-performance-summary]

本节数据适用于使用 CPS 激励模式的节点账户。在 CPS（按销售付费）模式下，节点根据来自适用司法辖区的被邀请人所产生的符合条件的交易手续费获得奖励。该接口返回 CPS 模式下节点账户的整体业绩概览。

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
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名**            | **类型**          | **描述** | **数据范围** |
| :------------------- | :--------------- | :------- | :------------- |
| uTime                | String           | 数据最近更新时间，Unix时间戳的毫秒数格式。 |
| inviteeCnt           | String           | 直客总数。 | CPS+TVB both |
| depAmt               | String           | 累计充值金额，单位为 `USDC`。 | CPS+TVB both |
| details              | Array of objects | 按业务类别拆分的明细，每个类别一条。 |
| > commissionCategory | String           | 返佣计算类别。<br>`SPOT`：现货<br>`DERIVATIVE`：衍生品<br>根据监管要求，来自 EEA 地区 OKX 用户的衍生品交易不适用返佣。来自符合资格地区的现货和衍生品交易返佣仍完全适用。 |
| > firstTraderCnt     | String           | 在选定周期内，加入或重新加入节点邀请关系后完成首次交易的被邀请人数。分别按 SPOT 和 DERIVATIVE 统计。只有产生有效返佣的交易才会计入；未产生返佣的交易不计入。<br><br>CPS firstTraderCnt 统计在选定周期内加入或重新加入节点邀请关系后完成首次交易，且该交易产生有效返佣的被邀请人。TVB validFirstTraderCnt 基于符合条件的交易量，不要求交易产生返佣。对于同一批被邀请人，CPS 统计值因此会系统性低于 TVB 统计值。两者不可比较。 | CPS+TVB both |
| > traderCnt          | String           | 选定周期内发生交易的去重被邀请人数，分别按 SPOT 和 DERIVATIVE 统计。同一被邀请人可能出现在多个类别中。 | CPS+TVB both |
| > vol                | String           | 节点的被邀请人在选定周期内产生的总交易量，分别按 SPOT 和 DERIVATIVE 统计。这是一般交易量，不是有效交易量或符合条件的交易量。 | CPS+TVB both |
| > commission         | String           | 选定窗口内该 `commissionCategory` 下的返佣，单位为 `USDC`。按窗口统计——区别于其他接口中的 `totalCommission`（生命周期累计）。 | CPS only |
