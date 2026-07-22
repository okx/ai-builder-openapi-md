---
site: turkey
lang: en
name: Get deposit history
method: GET
path: /api/v5/asset/deposit-history
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### Get deposit history

Retrieve the deposit records according to the currency, deposit status, and time range in reverse chronological order. The 100 most recent records are returned by default.<br>
Websocket API is also available, refer to [Deposit info channel](/docs-v5/en/#funding-account-websocket-deposit-info-channel).

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/asset/deposit-history`

> Request Example

```shell

GET /api/v5/asset/deposit-history

# Query deposit history from 2022-06-01 to 2022-07-01
GET /api/v5/asset/deposit-history?ccy=BTC&after=1654041600000&before=1656633600000

```

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Get deposit history
result = fundingAPI.get_deposit_history()
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                                                                   |
| :----- | :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy    | String | No       | Currency, e.g. `BTC`                                                                                                                                                                                                                                                                                          |
| depId  | String | No       | Deposit ID                                                                                                                                                                                                                                                                                                    |
| fromWdId  | String | No     | Internal transfer initiator's withdrawal ID<br>If the deposit comes from internal transfer, this field displays the withdrawal ID of the internal transfer initiator                                                                                                                                                                                                                                                                                            |
| txId   | String | No       | Hash record of the deposit                                                                                                                                                                                                                                                                                    |
| type   | String | No       | Deposit Type<br>`3`: internal transfer<br>`4`: deposit from chain                                                                                                                                                                                                                                             |
| state  | String | No       | Status of deposit  <br>`0`: waiting for confirmation<br>`1`: deposit credited  <br>`2`: deposit successful <br>`8`: pending due to temporary deposit suspension on this crypto currency<br>`11`: match the address blacklist<br>`12`: account or deposit is frozen<br>`13`: sub-account deposit interception<br>`14`: KYC limit<br> `17`: Pending response from Travel Rule vendor |
| after  | String | No       | Pagination of data to return records earlier than the requested ts, Unix timestamp format in milliseconds, e.g. `1654041600000`                                                                                                                                                                               |
| before | String | No       | Pagination of data to return records newer than the requested ts, Unix timestamp format in milliseconds, e.g. `1656633600000`                                                                                                                                                                                 |
| limit  | string | No       | Number of results per request. The maximum is `100`; The default is `100`                                                                                                                                                                                                                                     |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "actualDepBlkConfirm": "2",
        "amt": "1",
        "areaCodeFrom": "",
        "ccy": "USDT",
        "chain": "USDT-TRC20",
        "depId": "88****33",
        "from": "",
        "fromWdId": "",
        "state": "2",
        "to": "TN4hGjVXMzy*********9b4N1aGizqs",
        "ts": "1674038705000",
        "txId": "fee235b3e812********857d36bb0426917f0df1802"
    }
  ]
}
```

#### Response Parameters

| Parameter           | Type   | Description                                                                                                                                                                                                                                                                                                                   |
|---------------------| ------ |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                 | String | Currency                                                                                                                                                                                                                                                                                                                      |
| chain               | String | Chain name                                                                                                                                                                                                                                                                                                                    |
| amt                 | String | Deposit amount                                                                                                                                                                                                                                                                                                                |
| from                | String | Deposit account<br>If the deposit comes from an internal transfer, this field displays the account information of the internal transfer initiator, which can be a mobile phone number or email address (masked), and will return "" in other cases                                                                         |
| areaCodeFrom        | String | If `from` is a phone number, this parameter return area code of the phone number                                                                                                                                                                                                                                              |
| to                  | String | Deposit address<br>If the deposit comes from the on-chain, this field displays the on-chain address, and will return "" in other cases                                                                                                                                                                                        |
| txId                | String | Hash record of the deposit                                                                                                                                                                                                                                                                                                    |
| ts                  | String | The timestamp that the deposit record is created, Unix timestamp format in milliseconds, e.g. `1655251200000`                                                                                                                                                                                                                 |
| state               | String | Status of deposit<br>`0`: Waiting for confirmation<br>`1`: Deposit credited  <br>`2`: Deposit successful <br>`8`: Pending due to temporary deposit suspension on this crypto currency<br>`11`: Match the address blacklist<br>`12`: Account or deposit is frozen<br>`13`: Sub-account deposit interception<br>`14`: KYC limit |
| depId               | String | Deposit ID                                                                                                                                                                                                                                                                                                                    |
| fromWdId            | String | Internal transfer initiator's withdrawal ID<br>If the deposit comes from internal transfer, this field displays the withdrawal ID of the internal transfer initiator, and will return "" in other cases                                                                                                                       |
| actualDepBlkConfirm | String | The actual amount of blockchain confirmed in a single deposit                                                                                                                                                                                                                                                                 |

<aside class="notice">
About deposit state<br>
<b>Waiting for confirmation</b> is that the required number of blockchain confirmations has not been reached. <br>
<b>Deposit credited</b> is that there is sufficient number of blockchain confirmations for the currency to be credited to the account, but it cannot be withdrawn yet. <br>
<b>Deposit successful</b> means the crypto has been credited to the account and it can be withdrawn.
</aside>
