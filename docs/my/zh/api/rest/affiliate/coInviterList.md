---
site: my
lang: zh
name: 获取联合邀请人链接列表
method: GET
path: /api/v5/affiliate/co-inviter/list
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取联合邀请人链接列表 [affiliate-rest-api-get-co-inviter-link-list]

获取当前用户作为联合邀请人的链接列表。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/co-inviter/list`

> 请求示例

```shell
GET /api/v5/affiliate/co-inviter/list
```

#### 请求参数

| **参数名**  | **类型** | **是否必须** | **描述** |
| :--------- | :------- | :---------- | :------- |
| page       | String   | 否          | 1 起始的页码，非数字回退为 `1`。默认为 `1`。 |
| limit      | String   | 否          | 每页数量，限定在 `[1, 100]`。默认为 `100`。 |
| linkStatus | String   | 否          | 链接状态。<br>`normal`<br>`abnormal` |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "channelId": "31075853",
            "channelName": "MXCUS",
            "joinLink": "https://okx.com/join/MXCUS",
            "inviterCommissionRate": "0.0000",
            "coInviterCommissionRate": "0.1400",
            "inviteeDiscountRate": "0.1600",
            "parUserName": "12****23",
            "coUserName": "***",
            "isCompliant": true,
            "isDefault": false,
            "totalCommission": "0.032111",
            "commission24h": "0",
            "inviteeCnt": "1",
            "traderCnt": "1",
            "clickCnt": "1",
            "totalFee": "0",
            "cTime": "1773739123000",
            "channelAssessmentStatus": "valid",
            "inviterChannelStatus": "valid",
            "coInviterChannelStatus": "valid",
            "linkStatus": "normal"
        }
    ]
}
```

#### 返回参数

| **参数名**                | **类型**  | **描述** |
| :----------------------- | :------- | :------- |
| totalPage                | String   | 当前过滤条件与 `limit` 下的总页数，与 `data` 在响应中同级。 |
| channelId                | String   | 渠道 ID。 |
| channelName              | String   | 渠道名。 |
| joinLink                 | String   | 可分享的邀请 URL。 |
| inviterCommissionRate    | String   | 父级邀请人的返佣比例，小数形式。 |
| coInviterCommissionRate  | String   | 联合邀请人的返佣比例，小数形式（即调用方，因为该接口下调用方是联合邀请人）。 |
| inviteeDiscountRate      | String   | 该链接配置的直客返佣比例模板（小数形式），适用于通过该链接注册的直客。 |
| parUserName              | String   | 合作节点的用户名（部分脱敏，非 UID）。 |
| coUserName               | String   | 联合邀请人的用户名占位，恒为完全脱敏的 `***`（PII 完全隐藏，非 UID）。 |
| isCompliant              | Boolean  | 联合邀请人是否符合区域合规要求。<br>`true`：无限制<br>`false`：因 KYC 主体或司法辖区受限 |
| note                     | String   | 节点对该渠道的可选备注（自由文本，可能为 `""`）。 |
| isDefault                | Boolean  | 是否为默认联合邀请人链接。 |
| totalCommission          | String   | 累计返佣，单位为 `USDT`。 |
| commission24h            | String   | 滚动近 24 小时返佣，单位为 `USDT`。不受 `periodType` / `begin` / `end` 过滤影响。 |
| inviteeCnt               | String   | 直客数。 |
| traderCnt                | String   | 至少完成一次交易的直客数。 |
| clickCnt                 | String   | 邀请链接被点击次数。 |
| totalFee                 | String   | 累计服务费，单位为 `USDT`。 |
| cTime                    | String   | 链接创建时间，Unix时间戳的毫秒数格式。 |
| channelAssessmentStatus  | String   | 渠道考核结果（交易+邀请指标）。<br>`valid`：所有考核指标达标<br>`not_reach_trade`：交易指标未达标<br>`not_reach_invite`：邀请指标未达标<br>`not_reach_both`：两项均未达标 |
| inviterChannelStatus     | String   | 父级邀请人渠道合规状态。<br>`valid`：父级邀请人合规<br>`identity_invalid`：父级邀请人身份已失效<br>`level_downgraded`：父级邀请人等级已降为 0 |
| coInviterChannelStatus   | String   | 联合邀请人渠道合规状态（身份 × 考核 复合）。<br>`valid`<br>`identity_invalid`<br>`not_reach_assessment`<br>`identity_invalid_and_not_reach_assessment` |
| linkStatus               | String   | 链接状态。<br>`normal`<br>`abnormal` |
