---
site: turkey
lang: en
name: GET / Account rate limit
method: GET
path: /api/v5/trade/account-rate-limit
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Account rate limit

Get account rate limit related information. <br>

Only new order requests and amendment order requests will be counted towards this limit. For batch order requests consisting of multiple orders, each order will be counted individually. <br>

For details, please refer to [Fill ratio based sub-account rate limit](/docs-v5/en/#overview-rate-limits-fill-ratio-based-sub-account-rate-limit)

#### Rate Limit: 1 request per second

#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/account-rate-limit`

> Request Example

```shell
# Get the account rate limit
GET /api/v5/trade/account-rate-limit

```

#### Request Parameters

None

> Response Example

```json
{
   "code":"0",
   "data":[
      {
         "accRateLimit":"2000",
         "fillRatio":"0.1234",
         "mainFillRatio":"0.1234",
         "nextAccRateLimit":"2000",
         "ts":"123456789000"
      }
   ],
   "msg":""
}

```

#### Response Parameters

| Parameter | Type | Description |
| --- | --- | --- |
| fillRatio | String | Sub account fill ratio during the monitoring period. <br>Applicable to users with trading fee tier >= VIP 5; other users will receive `""`.<br>If there has been no trading activity on the account in the past 7 days, `""` will be returned.<br>If there is no executed volume during the monitoring period, `"0"` will be returned.<br>If there is executed volume but no order operation count during the monitoring period, `"9999"` will be returned.|
| mainFillRatio | String | Master account aggregated fill ratio during the monitoring period. <br>Applicable to users with trading fee tier >= VIP 5; other users will receive `""`.<br>If there has been no trading activity on the account in the past 7 days, `""` will be returned.<br>If there is no executed volume during the monitoring period, `"0"` will be returned.|
| accRateLimit | String | Current sub-account rate limit per 2 seconds |
| nextAccRateLimit | String | Expected sub-account rate limit (per 2 seconds) in the next monitoring period. <br>Applicable to users with trading fee tier >= VIP 5; other users will receive `""`.|
| ts | String | Data update timestamp <br>For users with trading fee tier >= VIP 5, the data will be generated daily at 08:00 am (UTC) <br>For users with trading fee tier < VIP 5, the current timestamp will be returned.|
