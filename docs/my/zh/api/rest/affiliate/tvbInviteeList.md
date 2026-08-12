---
site: my
lang: zh
name: 获取 TVB 被邀请人列表
method: GET
path: /api/v5/affiliate/tvb/invitee/list
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取 TVB 被邀请人列表 [affiliate-rest-api-get-tvb-invitee-list]

分页获取节点的 TVB（交易量返佣，Trading Volume Bonus）被邀请人列表——包含身份、手续费等级，以及统计窗口内的 TVB 交易量。所有金额单位均为 `USDC`。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/tvb/invitee/list`

> 请求示例

```shell
GET /api/v5/affiliate/tvb/invitee/list?periodType=last_30d&page=1&limit=100
```

#### 请求参数

| **参数名**   | **类型**          | **是否必须** | **描述** |
| :---------- | :---------------- | :---------- | :------- |
| periodType  | String            | 否          | 指标字段的统计窗口。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`this_week`<br>`today`<br>`total`<br>`custom`：自定义窗口，需配合 `begin` 与 `end` 使用。<br>默认为 `total`。<br>该窗口按活动时间对指标字段（`validVol` / `eligibleVol` / `validDepAmt` 及汇总聚合值）进行统计；不影响返回哪些被邀请人——被邀请人集合由 `feeTier` / `keyword` 决定。这些汇总聚合值覆盖整个过滤结果集，不随分页变化。 |
| begin       | String            | 条件必填     | 自定义统计窗口起始时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `end` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |
| end         | String            | 条件必填     | 自定义统计窗口结束时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `begin` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |
| feeTier     | Array of strings  | 否          | 按被邀请人手续费等级代码过滤：`0` = Regular，`10`–`18` = VIP1–VIP9。仅接受数字。此请求过滤器与返回的 `level` 字段不同，后者返回展示名称——见返回参数说明。 |
| tradeType   | String            | 否          | 交易量指标的交易产品类型（用于选择读取哪类产品的金额，而非筛选被邀请人）。<br>`ALL`<br>`SPOT`<br>`DERIVATIVE`<br>`BSC`<br>默认为 `ALL`。 |
| keyword     | String            | 否          | 按 UID 搜索被邀请人。 |
| orderBy     | String            | 否          | 排序字段。<br>`joinTime`<br>`validDepAmt`<br>`validVol`<br>默认为 `joinTime`。 |
| orderDir    | String            | 否          | 排序方向。<br>`asc`<br>`desc`<br>默认为 `desc`。 |
| page        | String            | 否          | 从 1 开始的页码。非数字值回退为 `1`。默认为 `1`。 |
| limit       | String            | 否          | 每页数量。限制在 `[1, 100]`。默认为 `100`。 |

自定义窗口规则与业绩概览接口一致：`periodType=custom` 需同时传 `begin` 和 `end`；窗口（`end − begin`）须在 1 到 90 天之间；`begin` 不得早于 180 天前。否则返回 `50076`。<br>
请求过滤器 `feeTier` 接受数字代码（`0`、`10`–`18`），而返回结果在 `level` 字段中返回本地化展示名称（`VIP 1`）。<br>
对无入金查看权限的调用方，`validDepAmt` 会被掩码为 `""`，与被邀请人详情接口的处理一致。<br>
`orderBy` / `orderDir` 按单次请求生效——翻页时每次调用都需带上，以保持跨页排序稳定。若在翻页过程中改变排序，跨页顺序行为未定义（可能出现重复或遗漏）。<br>
不暴露单个被邀请人的返佣。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "tradingVolBonus": "18.75",
            "validVol": "60700.00",
            "eligibleVol": "47400.00",
            "validDepAmt": "2246.56",
            "validInviteeCnt": "3",
            "validFirstTraderCnt": "1",
            "validTraderCnt": "2",
            "eligibleTraderCnt": "1",
            "invitees": [
                {
                    "uid": "855082962927686909",
                    "joinTime": "1782129625000",
                    "country": "NL",
                    "kycTime": "1782129625324",
                    "affiliateCode": "54022003",
                    "firstTradeTime": "1782216025000",
                    "level": "Regular User",
                    "validVol": "12500.00",
                    "eligibleVol": "9800.00",
                    "validDepAmt": "106.56",
                    "note": "VIP client from IG"
                },
                {
                    "uid": "855082962927686910",
                    "joinTime": "1781784025000",
                    "country": "DE",
                    "kycTime": "1781870425000",
                    "affiliateCode": "54022003",
                    "firstTradeTime": "1781956825000",
                    "level": "VIP 1",
                    "validVol": "48200.00",
                    "eligibleVol": "37600.00",
                    "validDepAmt": "2140.00",
                    "note": ""
                },
                {
                    "uid": "855082962927686911",
                    "joinTime": "1781525625000",
                    "country": "FR",
                    "kycTime": "",
                    "affiliateCode": "54022003",
                    "firstTradeTime": "",
                    "level": "Regular User",
                    "validVol": "0",
                    "eligibleVol": "0",
                    "validDepAmt": "0",
                    "note": ""
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名**           | **类型**          | **描述** |
| :------------------ | :--------------- | :------- |
| totalPage           | String           | 当前过滤条件与 `limit` 下的总页数。与响应中的 `data` 同级。 |
| tradingVolBonus     | String           | 选定窗口内累计的 TVB 返佣，单位为 `USDC`。为累计值——包含仍处于待发放/暂缓状态的金额。 |
| validVol            | String           | 选定窗口内的有效交易量——来自有效被邀请人的交易量，单位为 `USDC`。 |
| eligibleVol         | String           | 符合条件的交易量——有效交易量中产生返佣的子集（返佣计算基数），单位为 `USDC`。 |
| validDepAmt         | String           | 选定窗口内有效被邀请人的入金额，单位为 `USDC`。 |
| validInviteeCnt     | String           | 选定窗口内的去重有效被邀请人数。有效 = 与该节点存在有效邀请关系的用户。 |
| validFirstTraderCnt | String           | 首次交易发生在选定窗口内的去重有效被邀请人数（FTT）。每人至多计一次。 |
| validTraderCnt      | String           | 选定窗口内产生交易的去重有效被邀请人数。 |
| eligibleTraderCnt   | String           | 已交易并产生返佣的去重交易者数（`validTraderCnt` 的子集）。 |
| invitees            | Array of objects | 结果行。同一 `uid` 可能重复出现——窗口内每持有过一个手续费等级即一行，`level` 与各金额按该等级区间计。 |
| > uid               | String           | 被邀请人的 UID。 |
| > joinTime          | String           | 建立邀请关系的时间，Unix时间戳的毫秒数格式。 |
| > country           | String           | 被邀请人的国家 / 地区。 |
| > kycTime           | String           | KYC 认证时间，Unix时间戳的毫秒数格式。未认证时返回 `""`。 |
| > affiliateCode     | String           | 被邀请人注册时使用的节点邀请码。 |
| > firstTradeTime    | String           | 建立关系后的首次交易时间，Unix时间戳的毫秒数格式。未交易时返回 `""`。 |
| > level             | String           | 被邀请人的交易手续费等级展示名称，例如 `Regular User`、`VIP 1`–`VIP 9`。这是本地化展示名称，并非请求 `feeTier` 过滤器所用的数字代码——请勿将其作为过滤值回传。 |
| > validVol          | String           | 窗口内的交易量，单位为 `USDC`。 |
| > eligibleVol       | String           | 窗口内符合条件（可产生返佣）的交易量，单位为 `USDC`。 |
| > validDepAmt       | String           | 被邀请人在窗口内的入金额，单位为 `USDC`。对无入金查看权限的调用方返回 `""`（掩码）。 |
| > note              | String           | 节点对该被邀请人的私有备注。无备注时返回 `""`。 |
