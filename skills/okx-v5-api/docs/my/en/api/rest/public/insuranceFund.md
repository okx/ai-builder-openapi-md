---
site: my
lang: en
name: Get security fund
method: GET
path: /api/v5/public/insurance-fund
module: public
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### Get security fund

Get security fund balance information

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/insurance-fund`

> Request Example

```shell
GET /api/v5/public/insurance-fund?instType=SWAP&uly=BTC-USD
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Get security fund balance information
result = publicDataAPI.get_insurance_fund(
    instType="SWAP",
    uly="BTC-USD"
)
print(result)
```

#### Request Parameters

| Parameter      | Type    | Required        | Description                                                                                                                                        |
|:---------------|:--------|:----------------|:---------------------------------------------------------------------------------------------------------------------------------------------------|
| instType       | String  | Yes             | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`                                                                           |
| type           | String  | No              | Type<br>`liquidation_balance_deposit`<br>`bankruptcy_loss`<br><del>`platform_revenue`</del>(Deprecated, returns empty values. To be removed in a future update) <br><del>`adl`</del>(Deprecated, returns empty values. To be removed in a future update) <br>The default is `all type`                                     |
| instFamily     | String  | Conditional        | Instrument family<br>Required for `FUTURES`/`SWAP`/`OPTION` |
| ccy            | String  | Conditional        | Currency, only applicable to `MARGIN`                                                                                                              |
| before         | String  | No              | Pagination of data to return records newer than the requested `ts`                                                                                 |
| after          | String  | No              | Pagination of data to return records earlier than the requested `ts`                                                                               |
| limit          | String  | No              | Number of results per request. The maximum is `100`; The default is `100`                                                                          |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "adlType": "",
                    "amt": "1343.1308",
                    "balance": "1369179138.7489",
                    "ccy": "ETH",
                    "maxBal": "",
                    "maxBalTs": "",
                    "ts": "1704883083000",
                    "type": "liquidation_balance_deposit"
                }
            ],
            "instFamily": "ETH-USD",
            "instType": "OPTION",
            "total": "1369179138.7489"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter          | Type             | Description                                                                                         |
|:-------------------|:-----------------|:----------------------------------------------------------------------------------------------------|
| total              | String           | The total balance of security fund, in `USD`                                                         |
| instFamily         | String           | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                               |
| instType |	String |	Instrument type |
| details            | Array of objects | security fund data                                                                                 |
| > balance          | String           | The balance of security fund                                                                       |
| > amt              | String           | The change in the balance of security fund <br>Applicable when type is `liquidation_balance_deposit` or `bankruptcy_loss`                                                          |
| > ccy              | String           | The currency of security fund                                                                      |
| > type             | String           | The type of security fund<br>`liquidation_balance_deposit`<br>`bankruptcy_loss`<br><del>`platform_revenue`</del>(Deprecated, returns empty values)<br><del>`adl`</del>(Deprecated, returns empty values)                                                                          |
| > maxBal | String | ~~Maximum security fund balance in the past eight hours <br>Only applicable when type is `adl`~~(Deprecated, returns empty values) |
| > maxBalTs | String | ~~Timestamp when security fund balance reached maximum in the past eight hours, Unix timestamp format in milliseconds, e.g. `1597026383085` <br>Only applicable when type is `adl`~~(Deprecated, returns empty values) |
| > decRate | String | ~~Real-time security fund decline rate (compare balance and maxBal) <br>Only applicable when type is `adl`~~(Deprecated) |
| > adlType | String | ~~ADL related events <br>`rate_adl_start`: ADL begins due to high security fund decline rate <br>`bal_adl_start`: ADL begins due to security fund balance falling <br>`pos_adl_start`：ADL begins due to the volume of liquidation orders falls to a certain level (only applicable to premarket symbols) <br>`adl_end`: ADL ends <br>Only applicable when type is `adl`~~(Deprecated, returns empty values) |
| > ts               | String           | The update timestamp of security fund. Unix timestamp format in milliseconds, e.g. `1597026383085` |

<aside class="notice">
The `regular_update` type has been removed. The `adl` and `platform_revenue` types are deprecated and currently return empty values; they will be removed in a future update. The `amt` field presents the difference in security fund balance when the `type` is `liquidation_balance_deposit` or `bankruptcy_loss`, which is generated once per day around 08:00 am (UTC).
</aside>
