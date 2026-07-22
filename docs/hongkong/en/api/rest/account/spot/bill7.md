---
site: hongkong
lang: en
name: Get bills details (last 7 days)
method: GET
path: /api/v5/account/bills
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get bills details (last 7 days)

Retrieve the bills of the account. The bill refers to all transaction records that result in changing the balance of an account. Pagination is supported, and the response is sorted with the most recent first. This endpoint can retrieve data from the last 7 days.

#### Rate Limit: 5 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/account/bills`

> Request Example

```shell
GET /api/v5/account/bills

GET /api/v5/account/bills?instType=SPOT

```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Get bills details (last 7 days)
result = accountAPI.get_account_bills()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                |
|:----------| :----- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType  | String | No      | Instrument type<br>`SPOT`                                                                                                                                                  |
| ccy       | String | No      | Bill currency                                                                                                                                                              |
| type      | String | No      | Bill type<br>`1`: Transfer<br>`2`: Trade<br>`27`: Convert<br>`30`: Simple trade                                                                                  |
| subType   | String | No      | Bill subtype<br>`1`: Buy<br>`2`: Sell<br>`11`: Transfer in<br>`12`: Transfer out<br><br>`318`: Convert in<br>`319`: Convert out<br>`320`: Simple buy<br>`321`: Simple sell <br>`381`: Auto earn (auto lend)<br>`384`: Auto earn (USDG)<br>`392`: Auto staking<br>`395`: Auto earn (auto lend) interest transferred to bots<br>`396`: Auto staking interest transferred to bots|
| after     | String | No      | Pagination of data to return records earlier than the requested bill ID.                                                                                                   |
| before    | String | No      | Pagination of data to return records newer than the requested bill ID.                                                                                                     |
| begin     | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                 |
| end       | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                  |
| limit     | String | No      | Number of results per request. The maximum is `100`. The default is `100`.                                                                                                 |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [{
        "bal": "8694.2179403378290202",
        "balChg": "0.0219338232210000",
        "billId": "623950854533513219",
        "ccy": "USDT",
        "clOrdId": "",
        "earnAmt": "",
        "earnApr": "",
        "execType": "T",
        "fee": "-0.000021955779",
        "fillFwdPx": "",
        "fillIdxPx": "27104.1",
        "fillMarkPx": "",
        "fillMarkVol": "",
        "fillPxUsd": "",
        "fillPxVol": "",
        "fillTime": "1695033476166",
        "from": "",
        "instId": "BTC-USDT",
        "instType": "SPOT",
        "interest": "0",
        "mgnMode": "isolated",
        "notes": "",
        "ordId": "623950854525124608",
        "pnl": "0",
        "posBal": "0",
        "posBalChg": "0",
        "px": "27105.9",
        "subType": "1",
        "sz": "0.021955779",
        "tag": "",
        "to": "",
        "tradeId": "586760148",
        "ts": "1695033476167",
        "type": "2"
    }]
} 
```

#### Response Parameters

| Parameter   | Type   | Description                                                                                                                                                                                              |
|:------------| :----- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType    | String | Instrument type                                                                                                                                                                                          |
| billId      | String | Bill ID                                                                                                                                                                                                  |
| type        | String | Bill type                                                                                                                                                                                                |
| subType     | String | Bill subtype                                                                                                                                                                                             |
| ts          | String | The time when the balance complete update, Unix timestamp format in milliseconds, e.g.`1597026383085`                                                                                                    |
| balChg      | String | Change in balance amount at the account level                                                                                                                                                            |
| posBalChg   | String | Change in balance amount at the position level                                                                                                                                                           |
| bal         | String | Balance at the account level                                                                                                                                                                             |
| posBal      | String | Balance at the position level                                                                                                                                                                            |
| sz          | String | Quantity<br/>For `FUTURES`/`SWAP`/`OPTION`, it is fill quantity or position quantity, the unit is contract. The value is always positive.<br/>For other scenarios. the unit is account balance currency(`ccy`). |
| px          | String | Price which related to subType<br><li>Trade filled price for</li>`1`: Buy `2`: Sell                                                                                                                      |
| ccy         | String | Account balance currency                                                                                                                                                                                 |
| pnl         | String | Profit and loss                                                                                                                                                                                          |
| fee         | String | Fee<br>Negative number represents the user transaction fee charged by the platform.<br>Positive number represents rebate.                                                                                |
| earnAmt | String | Auto earn amount<br> Only applicable when type is 381 |
| earnApr | String | Auto earn APR<br> Only applicable when type is 381 |
| mgnMode     | String | Margin mode<br>`isolated` `cross` `cash`<br>When bills are not generated by trading, the field returns ""                                                                                              |
| instId      | String | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                           |
| ordId       | String | Order ID                                                                                                            |
| execType    | String | Liquidity taker or maker<br>`T`: taker<br>`M`: maker                                                                                                                                                     |
| from        | String | The remitting account<br>`6`: Funding account<br>`18`: Trading account<br>Only applicable to `transfer`. When bill type is not `transfer`, the field returns "".                                         |
| to          | String | The beneficiary account<br>`6`: Funding account<br>`18`: Trading account<br>Only applicable to `transfer`. When bill type is not `transfer`, the field returns "".                                       |
| notes       | String | Notes                                                                                                                                                                                                    |
| interest    | String | Interest                                                                                                                                                                                                 |
| tag         | String | Order tag                                                                                                                                                                                                |
| fillTime    | String | Last filled time                                                                                                                                                                                         |
| tradeId     | String | Last traded ID                                                                                                                                                                                           |
| clOrdId     | String | Client Order ID as assigned by the client<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                          |
| fillIdxPx   | String | Index price at the moment of trade execution<br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT.            |
| fillMarkPx  | String | Mark price when filled<br>Applicable to FUTURES/SWAP/OPTIONS, return "" for other instrument types                                                                                                       |
| fillPxVol   | String | Implied volatility when filled<br>Only applicable to options; return "" for other instrument types                                                                                                       |
| fillPxUsd   | String | Options price when filled, in the unit of USD<br>Only applicable to options; return "" for other instrument types                                                                                        |
| fillMarkVol | String | Mark volatility when filled<br>Only applicable to options; return "" for other instrument types                                                                                                          |
| fillFwdPx   | String | Forward price when filled<br>Only applicable to options; return "" for other instrument types                                                                                                            |
