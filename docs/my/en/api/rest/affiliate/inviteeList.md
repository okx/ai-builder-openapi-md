---
site: my
lang: en
name: Get invitee list
method: GET
path: /api/v5/affiliate/invitee/list
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get invitee list

Paginated invitee list with trading stats and KYC info.

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP Request

`GET /api/v5/affiliate/invitee/list`

> Request Example

```shell
GET /api/v5/affiliate/invitee/list?page=1&kycStatus=verified
```

#### Request Parameters

| **Parameter**       | **Type** | **Required** | **Description** |
| :------------------ | :------- | :----------- | :-------------- |
| page                | String   | No           | 1-indexed page number. Non-numeric values fall back to `1`. The default is `1`. |
| limit               | String   | No           | Items per page. Clamped to `[1, 100]`. The default is `100`. |
| periodType          | String   | No           | Stats window.<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>`custom`: pass `begin` and `end` to define a custom window. |
| begin               | String   | Conditional  | Custom stats-window start, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `end`. Inclusive. |
| end                 | String   | Conditional  | Custom stats-window end, Unix timestamp in millisecond format. Required when `periodType=custom`, together with `begin`. Inclusive. |
| keyword             | String   | No           | Search by UID or channel name. |
| commissionCategory  | String   | No           | Commission calculation category.<br>`SPOT`<br>`DERIVATIVE`<br>`BSC` |
| orderBy             | String   | No           | Sort field.<br>`cTime`<br>`depAmt`<br>`vol`<br>`fee`<br>`rebate`<br>The default is `cTime`. |
| orderDir            | String   | No           | Sort direction.<br>`asc`<br>`desc`<br>The default is `desc`. |
| kycStatus           | String   | No           | KYC status.<br>`unverified`<br>`verified` (passed at least KYC2) |
| subAffiliateUid     | String   | No           | Filter invitees under a specific sub-affiliate (external UID). |
| uid                 | String   | No           | External user UIDs for exact match — the same UID returned as `uid` in the response. Single UID or up to 100 UIDs, comma-separated, e.g. `835449167911924693,835449167911924700`. Unknown UIDs are silently skipped; if none resolve, an empty page is returned (never the full list). |
| joinTimeBegin       | String   | Conditional  | Filter lower bound on `joinTime` (the relationship-established time), Unix timestamp in millisecond format. Inclusive. Required when `joinTimeEnd` is supplied; the two must be sent together. Independent of the `periodType` / `begin` / `end` stats window. |
| joinTimeEnd         | String   | Conditional  | Filter upper bound on `joinTime` (the relationship-established time), Unix timestamp in millisecond format. Inclusive. Required when `joinTimeBegin` is supplied; the two must be sent together. Independent of the `periodType` / `begin` / `end` stats window. |

When `periodType=custom`, supply both `begin` and `end`. Supplying only one returns `50014`.<br>
For all other `periodType` values, server-defined windows are used and any `begin` / `end` passed alongside are ignored. The window between `begin` and `end` must not exceed 90 days. `begin` cannot be earlier than 180 days from now.

Supply both `joinTimeBegin` and `joinTimeEnd` or neither. `joinTimeBegin` equal to `joinTimeEnd` is a valid single-point range. The `joinTimeBegin` / `joinTimeEnd` span must not exceed 90 days, and `joinTimeBegin` cannot be earlier than 180 days from now.

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "5",
    "data": [
        {
            "uid": "835449167911924693",
            "country": "CN",
            "joinTime": "1777448564000",
            "firstTradeTime": "",
            "channelName": "X2UWA2T89",
            "rebateRate": "0.1600",
            "feeTierRank": "0",
            "kycStatus": "verified",
            "kycTime": "1777448563000",
            "depAmt": "0.0000000000",
            "totalVol": "0.0000000000",
            "totalFee": "0.0000000000",
            "totalCommission": "0.0000000000",
            "isCompliant": true
        }
    ]
}
```

#### Response Parameters

| **Parameter**     | **Type** | **Description** |
| :---------------- | :------- | :-------------- |
| totalPage         | String   | Total number of pages under the current filters and `limit`. Same level as `data` in the response. |
| uid               | String   | External user UID. |
| country           | String   | ISO 3166-1 alpha-2 country code of the invitee's residence, e.g. `KR`, `CN`. Empty string when residence is not set. |
| joinTime          | String   | Relationship established timestamp, Unix timestamp in millisecond format. |
| firstTradeTime    | String   | First trade timestamp, Unix timestamp in millisecond format. `""` if never traded. |
| channelName       | String   | Affiliate channel name used at registration. |
| rebateRate        | String   | Invitee's effective rebate rate from the active rebate calculation rule (in decimal), e.g. `0.1000` represents `10%`. |
| feeTierRank       | String   | Cross-category fee tier ranking integer (`0` = lowest, `13` = highest). Does not distinguish regular vs VIP — for the categorized label use `level` on [Get the invitee's detail](/docs-v5/en/#affiliate-rest-api-get-the-invitee-39-s-detail). |
| kycStatus         | String   | KYC status.<br>`unverified`<br>`verified` |
| kycTime           | String   | KYC2 verification timestamp, Unix timestamp in millisecond format. `""` if user has not passed KYC2. |
| depAmt            | String   | Total deposit amount, unit in `USDT`. |
| totalVol          | String   | Total trading volume, unit in `USDT`. |
| totalFee          | String   | Total trading fees, unit in `USDT`. |
| totalCommission   | String   | Total commission earned from this invitee, unit in `USDT`. |
| isCompliant       | Boolean  | Whether the invitee is permitted under regional compliance rules.<br>`true`: unrestricted<br>`false`: restricted due to KYC entity or jurisdiction (e.g. sanctioned region) |
