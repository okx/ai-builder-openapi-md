---
site: okex
lang: en
name: Get link list
method: GET
path: /api/v5/affiliate/link/list
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get link list

Paginated affiliate invite links with commission rates and stats.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/link/list`

> Request Example

```shell
GET /api/v5/affiliate/link/list
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| :------------ | :------- | :----------- | :-------------- |
| page          | String   | No           | 1-indexed page number. Non-numeric values fall back to `1`. The default is `1`. |
| limit         | String   | No           | Items per page. Clamped to `[1, 100]`. The default is `100`. |
| linkType      | String   | No           | Link kind filter.<br>`standard`: regular affiliate invite link<br>`co_inviter`: co-inviter shared link |
| linkStatus    | String   | No           | Link status.<br>`normal`<br>`abnormal` |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "channelId": "78295211",
            "channelName": "78295211",
            "joinLink": "https://okx.com/join/78295211",
            "linkType": "standard",
            "inviterCommissionRate": "0.2900",
            "coInviterCommissionRate": "",
            "inviteeDiscountRate": "0.0100",
            "inviteeCnt": "1",
            "traderCnt": "1",
            "totalCommission": "2.656307",
            "commission24h": "0.5",
            "cTime": "1773739123000",
            "isDefault": true,
            "linkStatus": "normal"
        }
    ]
}
```

#### Response Parameters

| **Parameter**            | **Type** | **Description** |
| :----------------------- | :------- | :-------------- |
| totalPage                | String   | Total number of pages under the current filters and `limit`. Same level as `data` in the response. |
| channelId                | String   | Unique channel/link ID. |
| channelName              | String   | User-defined link name. |
| joinLink                 | String   | Shareable invitation URL. |
| linkType                 | String   | Link kind.<br>`standard`: regular affiliate invite link<br>`co_inviter`: co-inviter shared link |
| inviterCommissionRate    | String   | Parent inviter's commission rate (the link owner's), in decimal. |
| coInviterCommissionRate  | String   | Co-inviter's commission rate (in decimal). Empty string for standard links. |
| inviteeDiscountRate      | String   | Invitee rebate rate template configured on this affiliate link (in decimal), applied to invitees registering via this link. |
| inviteeCnt               | String   | Number of invitees from this link. |
| traderCnt                | String   | Number of invitees who traded. |
| totalCommission          | String   | Cumulative commission, unit in `USDT`. |
| commission24h            | String   | Commission earned in the rolling past 24 hours, unit in `USDT`. Independent of `periodType` / `begin` / `end` filters. |
| cTime                    | String   | Link creation timestamp, Unix timestamp in millisecond format. |
| isDefault                | Boolean  | Whether this is the default link. |
| linkStatus               | String   | Link status.<br>`normal`<br>`abnormal` |
