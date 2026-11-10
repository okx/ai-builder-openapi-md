---
site: okex
lang: en
name: Get sub-affiliate list
method: GET
path: /api/v5/affiliate/sub-affiliate/list
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get sub-affiliate list

Paginated sub-affiliates under current user.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/sub-affiliate/list`

> Request Example

```shell
GET /api/v5/affiliate/sub-affiliate/list
```

#### Request Parameters

| **Parameter**      | **Type** | **Required** | **Description** |
| :----------------- | :------- | :----------- | :-------------- |
| page               | String   | No           | 1-indexed page number. Non-numeric values fall back to `1`. The default is `1`. |
| limit              | String   | No           | Items per page. Clamped to `[1, 100]`. The default is `100`. |
| keyword            | String   | No           | Search by sub-affiliate UID. |
| commissionCategory | String   | No           | Commission calculation category.<br>`SPOT`<br>`DERIVATIVE`<br>`BSC` |
| orderBy            | String   | No           | Sort field.<br>`cTime`<br>`depAmt`<br>`vol`<br>`fee`<br>`rebate`<br>The default sorts by `joinTime` (most recently joined first). |
| orderDir           | String   | No           | Sort direction.<br>`asc`<br>`desc`<br>The default is `desc`. |

The endpoint reports lifetime data.<br>
Sort is stable: ties on `orderBy` are broken by `subAffiliateUid` ascending. Safe to paginate large result sets without losing or duplicating rows.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "subAffiliateUid": "668418489887292061",
            "country": "CN",
            "joinTime": "1773739123000",
            "subAffiliateLevel": "2",
            "commissionRate": "0.3000",
            "isCompliant": true,
            "inviteeCnt": "8",
            "traderCnt": "3",
            "depAmt": "37.281133",
            "totalVol": "3618.561430",
            "totalFee": "1.628353",
            "totalCommission": "0.289847"
        }
    ]
}
```

#### Response Parameters

| **Parameter**     | **Type** | **Description** |
| :---------------- | :------- | :-------------- |
| totalPage         | String   | Total number of pages under the current filters and `limit`. Same level as `data` in the response. |
| subAffiliateUid   | String   | External UID of the sub-affiliate. |
| country           | String   | ISO 3166-1 alpha-2 country code of the sub-affiliate's residence, e.g. `CN`. Empty string when residence is not set. |
| joinTime          | String   | Timestamp the user was registered as a sub-affiliate, Unix timestamp in millisecond format. |
| subAffiliateLevel | String   | Depth of this sub-affiliate relative to the caller.<br>`2`: direct sub-affiliate (caller's L1 child)<br>`3`: indirect sub-affiliate, 1 level below a direct sub-affiliate |
| commissionRate    | String   | Sub-affiliate's commission rate, in decimal. |
| isCompliant       | Boolean  | Whether the sub-affiliate is permitted under regional compliance rules.<br>`true`: unrestricted<br>`false`: restricted due to KYC entity or jurisdiction (e.g. sanctioned region) |
| inviteeCnt        | String   | Direct invitees of the sub-affiliate. |
| traderCnt         | String   | Sub-affiliate's invitees who traded. |
| depAmt            | String   | Total deposit from invitees, unit in `USDT`. |
| totalVol          | String   | Total trading volume from invitees, unit in `USDT`. |
| totalFee          | String   | Total trading fees from invitees, unit in `USDT`. |
| totalCommission   | String   | Your commission earned from this sub-affiliate's invitees, unit in `USDT`. |
