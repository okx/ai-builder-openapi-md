---
site: my
lang: zh
name: 获取 TVB 分档返佣明细
method: GET
path: /api/v5/affiliate/tvb/tier-breakdown
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取 TVB 分档返佣明细 [affiliate-rest-api-get-tvb-bonus-breakdown-by-fee-tier]

本节数据适用于采用 TVB 激励模式的节点账户。在 TVB（交易量返佣，Trading Volume Bonus）模式下，奖励根据来自适用司法辖区的被邀请人所产生的符合条件的交易量计算。本接口按被邀请人的交易手续费等级拆分节点账户的 TVB。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/tvb/tier-breakdown`

> 请求示例

```shell
GET /api/v5/affiliate/tvb/tier-breakdown?periodType=last_30d
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
            "multiplier": "0.5",
            "tradingVolBonus": "15.5875",
            "ccy": "USDC",
            "uTime": "1784505600000",
            "tiers": [
                {
                    "level": "Regular User",
                    "validVol": "82000",
                    "eligibleVol": "60000",
                    "bonusRate": "0.00035",
                    "tradingVolBonus": "10.5"
                },
                {
                    "level": "VIP 1",
                    "validVol": "21500",
                    "eligibleVol": "20000",
                    "bonusRate": "0.000305",
                    "tradingVolBonus": "3.05"
                },
                {
                    "level": "VIP 2",
                    "validVol": "10000",
                    "eligibleVol": "10000",
                    "bonusRate": "0.000255",
                    "tradingVolBonus": "1.275"
                },
                {
                    "level": "VIP 3",
                    "validVol": "6100",
                    "eligibleVol": "5000",
                    "bonusRate": "0.00019",
                    "tradingVolBonus": "0.475"
                },
                {
                    "level": "VIP 4",
                    "validVol": "2400",
                    "eligibleVol": "2400",
                    "bonusRate": "0.000175",
                    "tradingVolBonus": "0.21"
                },
                {
                    "level": "VIP 5",
                    "validVol": "1300",
                    "eligibleVol": "1000",
                    "bonusRate": "0.000155",
                    "tradingVolBonus": "0.0775"
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名**         | **类型**          | **描述** | **数据范围** |
| :---------------- | :--------------- | :------- | :------------- |
| multiplier        | String           | 节点当前返佣倍率，以小数比率表示，例如 `0.5` 表示 50%。不受统计窗口影响。 |
| tradingVolBonus   | String           | 选定窗口内累计的 TVB 返佣，单位为 `USDC`。为节点层级的总额——等于 `tiers[].tradingVolBonus` 之和。包含仍处于待发放或暂停状态的金额，可能与实际发放金额不一致。 | TVB only |
| ccy               | String           | 结算币种，恒为 `USDC`。 |
| uTime             | String           | 数据最近更新时间，Unix时间戳的毫秒数格式。暂无数据时返回 `""`。 |
| tiers             | Array of objects | 每个被邀请人手续费等级一行。 |
| > level           | String           | 等级展示名称。<br>`Regular User`<br>`VIP 1`<br>`VIP 2`<br>`VIP 3`<br>`VIP 4`<br>`VIP 5` |
| > validVol        | String           | 选定窗口内的有效交易量——来自有效被邀请人的交易量，单位为 `USDC`。 | TVB only |
| > eligibleVol     | String           | 符合条件的交易量——有效交易量中产生返佣的子集（返佣计算基数），单位为 `USDC`。 | TVB only |
| > bonusRate       | String           | 该等级在本项目中的固定返佣费率，以小数表示。例如 `0.00035`。 |
| > tradingVolBonus | String           | 选定窗口内该等级 `eligibleVol` 产生并已应用 `multiplier` 的交易量返佣，单位为 `USDC`。 | TVB only |

交易量返佣项目不向 VIP 6 及以上的被邀请人开放，因此分档明细最高至 `VIP 5`。
