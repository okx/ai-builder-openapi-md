---
site: us
lang: en
name: Get premium history
method: GET
path: /api/v5/public/premium-history
module: public
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### Get premium history

It will return premium data in the past 6 months.

#### Rate Limit: 20 requests per 2 seconds 
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/premium-history`

> Request Example

```shell
GET /api/v5/public/premium-history?instId=BTC-USDT-SWAP
```

```python
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                      |
|:-----------| :----- |:---------|:---------------------------------------------------------------------------------|
| instId     | String | Yes      | Instrument ID, e.g. `BTC-USDT-SWAP`<br>Applicable to `SWAP`                      |
| after     | String | No       | Pagination of data to return records earlier than the requested ts(not included) |
| before     | String | No      | Pagination of data to return records newer than the requested ts(not included)   |
| limit     | String | No      | Number of results per request. The maximum is `100`. The default is `100`.       |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "instId": "BTC-USDT-SWAP",
            "premium": "0.0000578896878167",
            "ts": "1713925924000"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                   |
|:--------------| :------- |:----------------------------------------------------------------------------------|
| instId        | String   | Instrument ID, e.g. `BTC-USDT-SWAP`                                               |
| premium | String | Premium index<br> formula: [Max (0, Impact bid price – Index price) – Max (0, Index price – Impact ask price)] / Index price |
| ts            | String   | Data generation time, Unix timestamp format in milliseconds, e.g. `1597026383085` |
