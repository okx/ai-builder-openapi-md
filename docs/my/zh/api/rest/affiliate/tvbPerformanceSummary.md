---
site: my
lang: zh
name: 获取 TVB 业绩概览
method: GET
path: /api/v5/affiliate/tvb/performance/summary
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取 TVB 业绩概览 [affiliate-rest-api-get-tvb-performance-summary]

本节数据适用于采用 TVB 激励模式的节点账户。在 TVB（交易量返佣，Trading Volume Bonus）模式下，奖励根据来自适用司法辖区的被邀请人所产生的符合条件的交易量计算。本接口返回 TVB 模式下节点账户的整体业绩概览。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/tvb/performance/summary`

> 请求示例

```shell
GET /api/v5/affiliate/tvb/performance/summary?periodType=last_30d
```

#### 请求参数

| **参数名**  | **类型**  | **是否必须** | **描述** |
| :--------- | :------- | :---------- | :------- |
| periodType | String   | 否          | 指标字段的统计窗口。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`this_week`<br>`today`<br>`total`<br>`custom`：自定义窗口，需配合 `begin` 与 `end` 使用。<br>默认为 `total` |
| begin      | String   | 条件必填     | 自定义统计窗口起始时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `end` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |
| end        | String   | 条件必填     | 自定义统计窗口结束时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `begin` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |

当 `periodType=custom` 时，需同时传 `begin` 和 `end`，仅传一个会返回 `50014`。<br>
其他 `periodType` 值使用服务端预设窗口，与之同时传入的 `begin` / `end` 将被忽略。<br>
当省略 `periodType` 时：若同时传入 `begin` 和 `end`，则回退为 `custom`；否则为 `total`。<br>
自定义窗口（`end − begin`）须在 1 到 90 天之间。<br>
`begin` 不得早于 180 天前——仅提供最近 180 天的数据。<br>
`multiplier`、`ccy` 与 `uTime` 不受统计窗口影响——无论 `periodType` 为何，它们都反映当前配置 / 数据快照。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "tradingVolBonus": "15.60",
            "validVol": "125000.00",
            "eligibleVol": "98400.00",
            "validInviteeCnt": "102",
            "validTraderCnt": "26",
            "eligibleTraderCnt": "19",
            "validDepAmt": "1420.113200000000000000",
            "validFirstTraderCnt": "17",
            "validFirstDepositorCnt": "12",
            "multiplier": "0.5",
            "ccy": "USDC",
            "uTime": "1777541513000"
        }
    ]
}
```

#### 返回参数

| **参数名**             | **类型** | **描述** | **数据范围** |
| :--------------------- | :------- | :------- | :------------- |
| tradingVolBonus        | String   | 选定窗口内累计的 TVB 返佣，单位为 `USDC`。为累计值——包含仍处于待发放/暂缓状态的金额。包含仍处于待发放或暂停状态的金额，可能与实际发放金额不一致。 | TVB only |
| validVol               | String   | 选定窗口内的有效交易量——来自有效被邀请人的交易量，单位为 `USDC`。 | TVB only |
| eligibleVol            | String   | 符合条件的交易量——有效交易量中产生返佣的子集（返佣计算基数），单位为 `USDC`。 | TVB only |
| validInviteeCnt        | String   | 选定窗口内的去重有效被邀请人数。有效 = 与该节点存在有效邀请关系的用户。 | CPS+TVB both |
| validTraderCnt         | String   | 选定窗口内产生交易的去重有效被邀请人数。 | TVB only |
| eligibleTraderCnt      | String   | 已交易并产生返佣的去重交易者数（`validTraderCnt` 的子集）。 | TVB only |
| validDepAmt            | String   | 选定窗口内有效被邀请人的入金额，单位为 `USDC`。 | CPS+TVB both |
| validFirstTraderCnt    | String   | 首次交易发生在选定窗口内的去重有效被邀请人数（FTT）。每人至多计一次。<br><br>CPS firstTraderCnt 统计在选定周期内加入或重新加入节点邀请关系后完成首次交易，且该交易产生有效返佣的被邀请人。TVB validFirstTraderCnt 基于符合条件的交易量，不要求交易产生返佣。对于同一批被邀请人，CPS 统计值因此会系统性低于 TVB 统计值。两者不可比较。 | TVB only |
| validFirstDepositorCnt | String   | 首次入金发生在选定窗口内的去重有效被邀请人数（FTD）。每人至多计一次。 | TVB only |
| multiplier             | String   | 节点当前返佣倍率，以小数比率表示，例如 `0.5` 表示 50%。不受统计窗口影响。 |
| ccy                    | String   | 结算币种，恒为 `USDC`。 |
| uTime                  | String   | 数据最近更新时间，Unix时间戳的毫秒数格式。暂无数据时返回 `""`。 |
