---
site: my
lang: zh
name: 获取 TVB 邀请链接列表
method: GET
path: /api/v5/affiliate/tvb/link/list
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取 TVB 邀请链接列表 [affiliate-rest-api-get-tvb-link-list]

本节数据适用于采用 TVB 激励模式的节点账户。在 TVB（交易量返佣，Trading Volume Bonus）模式下，奖励根据来自适用司法辖区的被邀请人所产生的符合条件的交易量计算。本接口返回与节点账户关联的 TVB 邀请链接。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/tvb/link/list`

> 请求示例

```shell
GET /api/v5/affiliate/tvb/link/list?linkType=standard
```

#### 请求参数

| **参数名**  | **类型**  | **是否必须** | **描述** |
| :--------- | :------- | :---------- | :------- |
| periodType | String   | 否          | 指标字段的统计窗口。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`this_week`<br>`today`<br>`total`<br>`custom`：自定义窗口，需配合 `begin` 与 `end` 使用。<br>默认为 `total` |
| begin      | String   | 条件必填     | 自定义统计窗口起始时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `end` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |
| end        | String   | 条件必填     | 自定义统计窗口结束时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `begin` 同时传入。包含端点。用于自定义窗口——`periodType=custom`，或省略 `periodType` 且同时传入 `begin` 与 `end`。 |
| page       | String   | 否          | 从 1 开始的页码。非数字值回退为 `1`。默认为 `1`。 |
| limit      | String   | 否          | 每页数量。限制在 `[1, 100]`。默认为 `100`。 |
| linkType   | String   | 否          | 链接类型过滤器。<br>`standard`：标准邀请链接<br>`co_inviter`：共同邀请人链接<br>省略则返回全部类型。 |
| linkStatus | String   | 否          | 链接状态。<br>`normal`<br>`abnormal` |

当 `periodType=custom` 时，需同时传 `begin` 和 `end`，仅传一个会返回 `50014`。<br>
其他 `periodType` 值使用服务端预设窗口，与之同时传入的 `begin` / `end` 将被忽略。<br>
当省略 `periodType` 时：若同时传入 `begin` 和 `end`，则回退为 `custom`；否则为 `total`。<br>
自定义窗口（`end − begin`）须在 1 到 90 天之间；`begin` 不得早于 180 天前。否则返回 `50076`。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "3",
    "data": [
        {
            "channelId": "245012001",
            "channelName": "Summer Campaign",
            "linkStatus": "normal",
            "joinLink": "https://www.okx.com/join/245012001",
            "note": "IG bio link",
            "validInviteeCnt": "58",
            "validTraderCnt": "24",
            "eligibleTraderCnt": "21",
            "validVol": "96500.00",
            "eligibleVol": "84200.00",
            "tradingVolBonus": "12.63",
            "cTime": "1782129625000"
        },
        {
            "channelId": "245012002",
            "channelName": "Telegram Group",
            "linkStatus": "abnormal",
            "joinLink": "https://www.okx.com/join/245012002",
            "note": "",
            "validInviteeCnt": "0",
            "validTraderCnt": "0",
            "eligibleTraderCnt": "0",
            "validVol": "0",
            "eligibleVol": "0",
            "tradingVolBonus": "0",
            "cTime": "1779537625000"
        }
    ]
}
```

#### 返回参数

| **参数名**         | **类型** | **描述** | **数据范围** |
| :---------------- | :------- | :------- | :------------- |
| totalPage         | String   | 当前过滤条件与 `limit` 下的总页数。与响应中的 `data` 同级。 |
| channelId         | String   | 链接的渠道 ID。 |
| channelName       | String   | 链接展示名称。 |
| linkStatus        | String   | 链接状态。<br>`normal`<br>`abnormal` |
| joinLink          | String   | 该链接可分享的邀请 URL。 |
| note              | String   | 用户对该链接自定义的备注。无备注时返回 `""`。 |
| validInviteeCnt   | String   | 归属该链接的去重有效被邀请人数。无则为 `0`。 | CPS+TVB both |
| validTraderCnt    | String   | 该链接上的去重有效交易者数。无则为 `0`。 | TVB only |
| eligibleTraderCnt | String   | 去重的符合条件交易者数——有效交易者中产生返佣的子集。无则为 `0`。 | TVB only |
| validVol          | String   | 该链接上的有效交易量，单位为 `USDC`。无则为 `0`。 | TVB only |
| eligibleVol       | String   | 符合条件的交易量——有效交易量中产生返佣的子集（返佣计算基数），单位为 `USDC`。无则为 `0`。 | TVB only |
| tradingVolBonus   | String   | 该链接累计的 TVB 返佣，单位为 `USDC`。无则为 `0`。 | TVB only |
| cTime             | String   | 链接创建时间，Unix时间戳的毫秒数格式。不可用时返回 `""`。 |
