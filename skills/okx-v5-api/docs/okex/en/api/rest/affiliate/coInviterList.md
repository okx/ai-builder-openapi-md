---
site: okex
lang: en
name: Get co-inviter link list
method: GET
path: /api/v5/affiliate/co-inviter/list
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get co-inviter link list

Co-inviter links where the authenticated user is the co-inviter.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/co-inviter/list`

> Request Example

```shell
GET /api/v5/affiliate/co-inviter/list
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| page          | String   | No           | 1-indexed page number. Non-numeric values fall back to `1`. The default is `1`. |
| limit         | String   | No           | Items per page. Clamped to `[1, 100]`. The default is `100`. |
| linkStatus    | String   | No           | Link status.<br>`normal`<br>`abnormal` |

> Response Example

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

#### Response Parameters

| **Parameter**             | **Type** | **Description** |
| :------------------------ | :------- | :-------------- |
| totalPage                 | String   | Total number of pages under the current filters and `limit`. Same level as `data` in the response. |
| channelId                 | String   | Channel ID. |
| channelName               | String   | Channel name. |
| joinLink                  | String   | Shareable invitation URL. |
| inviterCommissionRate     | String   | Parent inviter's commission rate, in decimal. |
| coInviterCommissionRate   | String   | Co-inviter's commission rate, in decimal (the caller's, since the caller is the co-inviter on this endpoint). |
| inviteeDiscountRate       | String   | Invitee rebate rate template configured on this affiliate link (in decimal), applied to invitees registering via this link. |
| parUserName               | String   | Partner affiliate's username string (partially masked, not a UID). |
| coUserName                | String   | Co-inviter's username placeholder, always fully masked as `***` (PII fully hidden, not a UID). |
| isCompliant               | Boolean  | Whether the co-inviter is permitted under regional compliance rules.<br>`true`: unrestricted<br>`false`: restricted due to KYC entity or jurisdiction |
| note                      | String   | Optional channel note set by the affiliate (free-form text, may be empty `""`). |
| isDefault                 | Boolean  | Whether this is the default co-inviter link. |
| totalCommission           | String   | Total commission, unit in `USDT`. |
| commission24h             | String   | Commission earned in the rolling past 24 hours, unit in `USDT`. Independent of `periodType` / `begin` / `end` filters. |
| inviteeCnt                | String   | Number of invitees. |
| traderCnt                 | String   | Number of invitees who placed at least one trade. |
| clickCnt                  | String   | Number of times the affiliate link was clicked (click count). |
| totalFee                  | String   | Total service fees, unit in `USDT`. |
| cTime                     | String   | Link creation timestamp, Unix timestamp in millisecond format. |
| channelAssessmentStatus   | String   | Channel assessment result (trade + invite metrics).<br>`valid`: all assessment metrics met<br>`not_reach_trade`: trade metric unmet<br>`not_reach_invite`: invite metric unmet<br>`not_reach_both`: both unmet |
| inviterChannelStatus      | String   | Parent inviter's channel compliance.<br>`valid`: parent inviter is compliant<br>`identity_invalid`: parent inviter's identity has been revoked<br>`level_downgraded`: parent inviter's level has been reduced to 0 |
| coInviterChannelStatus    | String   | Co-inviter's channel compliance (compound: identity × assessment).<br>`valid`<br>`identity_invalid`<br>`not_reach_assessment`<br>`identity_invalid_and_not_reach_assessment` |
| linkStatus                | String   | Link status.<br>`normal`<br>`abnormal` |
