---
site: my
lang: en
name: Get positions history
method: GET
path: /api/v5/account/positions-history
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Get positions history

Retrieve the updated position data for the last 3 months. Return in reverse chronological order using utime. Getting positions history is supported under Portfolio margin mode since **04:00 AM (UTC) on November 11, 2024**.

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/positions-history`

> Request Example

```shell
GET /api/v5/account/positions-history
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get positions history
result = accountAPI.get_positions_history()
print(result)
```

#### Request Parameters

| Parameter  | Type | Required | Description                                                                                                                                                                                                    |
| :------- | :----- |:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType | String | No       | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instId   | String | No        | Instrument ID, e.g. `BTC-USD-SWAP`                                                                                                                                                                             |
| mgnMode   | String | No        | Margin mode<br>`cross` `isolated`                                                                                                                                                                              |
| type   | String | No        | The type of latest close position<br>`1`: Close position partially;`2`：Close all;`3`：Liquidation;`4`：Partial liquidation; `5`：ADL - position not fully closed; `6`：ADL - position fully closed<br>It is the latest type if there are several types for the same position. |
| posId       | String  | No | Position ID. There is attribute expiration. The posId will be expired if it is more than 30 days after the last full close position, then position will use new posId.                                         |
| after   | String | No        | Pagination of data to return records earlier than the requested `uTime`, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                           |
| before   | String | No        | Pagination of data to return records newer than the requested `uTime`, Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                             |
| limit   | String | No        | Number of results per request. The maximum is 100. The default is 100. All records that have the same `uTime` will be returned at the current request                                                          |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "cTime": "1654177169995",
            "ccy": "BTC",
            "closeAvgPx": "29786.5999999789081085",
            "closeTotalPos": "1",
            "instId": "BTC-USD-SWAP",
            "instType": "SWAP",
            "lever": "10.0",
            "mgnMode": "cross",
            "openAvgPx": "29783.8999999995535393",
            "openMaxPos": "1",
            "realizedPnl": "0.001",
            "fee": "-0.0001",
            "fundingFee": "0",
            "liqPenalty": "0",
            "pnl": "0.0011",
            "pnlRatio": "0.000906447858888",
            "posId": "452587086133239818",
            "posSide": "long",
            "direction": "long",
            "triggerPx": "",
            "type": "1",
            "uTime": "1654177174419",
            "uly": "BTC-USD",
            "nonSettleAvgPx":"",
            "settledPnl":""
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| Parameter               | Type       | Description                                                                                                                                                                                                                                                                                                                                                                   |
|:------------------------|:-----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType                | String     | Instrument type<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS` |
| instId                  | String     | Instrument ID                                                                                                                                                                                                                                                                                                                                                                 |
| mgnMode                 | String     | Margin mode<br>`cross` `isolated`                                                                                                                                                                                                                                                                                                                                             |
| type                    | String     | The type of latest close position<br>`1`：Close position partially;`2`：Close all;`3`：Liquidation;`4`：Partial liquidation; `5`：ADL; <br>It is the latest type if there are several types for the same position.                                                                                                                                                                 |
| cTime                   | String     | Created time of position                                                                                                                                                                                                                                                                                                                                                      |
| uTime                   | String     | Updated time of position                                                                                                                                                                                                                                                                                                                                                      |
| openAvgPx               | String     | Average price of opening position<br>Under cross-margin mode, the entry price of expiry futures will update at settlement to the last settlement price, and when the position is opened or increased.                                                                                                                                                                         |
| nonSettleAvgPx          | String     | Non-settlement entry price<br>The non-settlement entry price only reflects the average price at which the position is opened or increased.<br>Only applicable to `cross` `FUTURES`                                                                                                                                                                                            |
| closeAvgPx              | String     | Average price of closing position                                                                                                                                                                                                                                                                                                                                             |
| posId                   | String     | Position ID                                                                                                                                                                                                                                                                                                                                                                   |
| openMaxPos              | String     | Max quantity of position                                                                                                                                                                                                                                                                                                                                                      |
| closeTotalPos           | String     | Position's cumulative closed volume                                                                                                                                                                                                                                                                                                                                           |
| realizedPnl             | String     | Realized profit and loss<br>Only applicable to `FUTURES`/`SWAP`/`OPTION`<br>`realizedPnl`=`pnl`+`fee`+`fundingFee`+`liqPenalty`+`settledPnl`                                                                                                                                                                                                                                  |
| settledPnl               | String     | Accumulated settled profit and loss (calculated by settlement price)<br>Only applicable to `cross` `FUTURES`                                                                                                                                                                                                                                                                              |
| pnlRatio                | String     | Realized P&L ratio                                                                                                                                                                                                                                                                                                                                                            |
| fee                     | String     | Accumulated fee<br>Negative number represents the user transaction fee charged by the platform.Positive number represents rebate.                                                                                                                                                                                                                                             |
| fundingFee              | String     | Accumulated funding fee                                                                                                                                                                                                                                                                                                                                                       |
| liqPenalty              | String     | Accumulated liquidation penalty. It is negative when there is a value.                                                                                                                                                                                                                                                                                                        |
| pnl                     | String     | Profit and loss (excluding the fee).                                                                                                                                                                                                                                                                                                                                                               |
| posSide                 | String     | Position mode side<br> `long`: Hedge mode long<br> `short`: Hedge mode short<br> `net`: Net mode                                                                                                                                                                                                                                                                              |
| lever                   | String     | Leverage                                                                                                                                                                                                                                                                                                                                                                      |
| direction               | String     | Direction: `long` `short`<br>Only applicable to `MARGIN/FUTURES/SWAP/OPTION`                                                                                                                                                                                                                                                                                                  |
| triggerPx               | String     | trigger mark price. There is value when `type` is equal to `3`, `4` or `5`. It is "" when `type` is equal to `1` or `2`                                                                                                                                                                                                                                                       |
| uly                     | String     | Underlying                                                                                                                                                                                                                                                                                                                                                                    |
| ccy                     | String     | Currency used for margin                                                                                                                                                                                                                                                                                                                                                      |
