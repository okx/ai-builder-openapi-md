---
site: turkey
lang: en
name: Get the invitee's detail
method: GET
path: /api/v5/affiliate/invitee/detail
module: affiliate
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get the invitee's detail

#### Rate Limit: 3 requests per second
#### Rate limit rule: User ID
#### HTTP request

`GET /api/v5/affiliate/invitee/detail`

> Request sample

```shell
GET /api/v5/affiliate/invitee/detail?uid=11111111&periodType=last_30d
```

#### Request Parameters

| Parameter | Type  | Required| Description                                                                                                                                               |
| :------ | :----- | :------- |:----------------------------------------------------------------------------------------------------------------------------------------------------------|
| uid      | String | Yes      | UID of the invitee. Only applicable to the UID of invitee master account. <br/> The data returned covers invitee master account and invitee sub-accounts. |
| periodType | String | No     | Stats window for `volPeriod`.<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>When omitted, `volPeriod` is not returned. `custom` is not supported on this endpoint; `custom` or any unknown value returns `51000`. |

> Returned results

```json
{
    "msg": "",
    "code": "0",
    "data": [
        {
            "accFee": "0",
            "affiliateCode": "HIIIIII",
            "depAmt": "0",
            "wdAmt": "0",
            "firstTradeTime": "",
            "inviteeLevel": "2",
            "inviteeRebateRate": "0.39",
            "joinTime": "1712546713000",
            "kycTime": "",
            "level": "Lv1",
            "region": "Vietnam",
            "totalCommission": "0",
            "volMonth": "0",
            "totalVol": "0",
            "volPeriod": "1234.56"
        }
    ]
}
```

#### Response parameters

| **Parameter name** | **Type** | **Description** |
|:-------------------| :------- |:------------------------------------------------------------------------------------------------------------------ |
| inviteeLevel       | String   | Invitee's relative level to the affiliate<br>If the user is a invitee, the level will be `2`.                     |
| joinTime           | String     | Timestamp that the rebate relationship is established, Unix timestamp in millisecond format, e.g. `1597026383085` |
| inviteeRebateRate  | String     | Self rebate rate of the invitee (in decimal), e.g. `0.01` represents `1%`                                             |
| totalCommission    | String     | Total commission earned from the invitee, unit in `USDT` |
| firstTradeTime     | String  | Timestamp that the first trade is completed after the latest rebate relationship is established with the parent affiliate<br/> Unix timestamp in millisecond format, e.g. 1597026383085<br/>If user has not traded, "" will be returned |
| level              | String   | Invitee trading fee level, e.g. Lv1                                                                                                                                                                                                     |
| depAmt             | String   | Accumulated amount of deposit in USDT<br/>If user has not deposited, 0 will be returned |
| wdAmt              | String   | Accumulated amount of withdrawal in USDT<br/>If user has not withdrawn, 0 will be returned |
| volMonth           | String   | Accumulated Trading volume in the current month in USDT<br/>If user has not traded, 0 will be returned |
| totalVol           | String   | Lifetime accumulated trading volume in USDT<br/>If user has not traded, 0 will be returned |
| volPeriod          | String   | Trading volume within the selected `periodType` window, unit in `USDT`. Only returned when `periodType` is supplied in the request; omitted otherwise. If the user has not traded in the window, `0` is returned. |
| accFee             | String   | Accumulated Amount of trading fee in USDT<br/>If there is no any fee, 0 will be returned |
| kycTime            | String   | KYC2 verification time. Unix timestamp in millisecond format and the precision is in day<br/>If user has not passed KYC2, "" will be returned                                                                                           |
| region             | String   | User country or region. e.g. "United Kingdom"                                                                                                                                                                                           |
| affiliateCode      | String   | Affiliate invite code that the invitee registered/recalled via                                                                                                                                                 |
