---
site: turkey
lang: en
name: GET / Transaction details (last 3 days)
method: GET
path: /api/v5/trade/fills
module: trade
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / Transaction details (last 3 days)

Retrieve recently-filled transaction details in the last 3 day.

#### Rate Limit: 60 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`GET /api/v5/trade/fills`

> Request Example

```shell
GET /api/v5/trade/fills

```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Retrieve recently-filled transaction details
result = tradeAPI.get_fills()
print(result)
```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                              |
|:-----------| :----- | :------- |:-----------------------------------------------------------------------------------------|
| instType   | String | No       | Instrument type<br>`SPOT`                                                                |
| instId     | String | No       | Instrument ID, e.g. `BTC-USDT`                                                           |
| ordId      | String | No       | Order ID                                                                                 |
| after      | String | No       | Pagination of data to return records earlier than the requested `billId`                 |
| before     | String | No       | Pagination of data to return records newer than the requested `billId`                   |
| begin      | String | No       | Filter with a begin timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085` |
| end        | String | No       | Filter with an end timestamp. Unix timestamp format in milliseconds, e.g. `1597026383085`  |
| limit      | String | No       | Number of results per request. The maximum is `100`; The default is `100`                |

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "side": "buy",
            "fillSz": "0.00192834",
            "fillPx": "51858",
            "fillPxVol": "",
            "fillFwdPx": "",
            "fee": "-0.00000192834",
            "fillPnl": "0",
            "ordId": "680800019749904384",
            "feeRate": "-0.001",
            "instType": "SPOT",
            "fillPxUsd": "",
            "instId": "BTC-USDT",
            "clOrdId": "",
            "posSide": "net",
            "billId": "680800019754098688",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                     |
|:--------------| :------- | :----------------------------------------------------------- |
| instType      | String   | Instrument type                                                     |
| instId        | String   | Instrument ID                                                   |
| tradeId       | String   | Last trade ID                                                   |
| ordId         | String   | Order ID                                                       |
| clOrdId       | String   | Client Order ID as assigned by the client                                                  |
| billId        | String   | Bill ID                                                       |
| tag           | String   | Order tag                                                     |
| fillPx        | String   | Last filled price                                                 |
| fillSz        | String   | Last filled quantity                                                 |
| fillIdxPx     | String   | Index price at the moment of trade execution <br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT.         |
| fillPnl       | String   | Last filled profit and loss, applicable to orders which have a trade and aim to close position. It always is 0 in other conditions |
| fillPxVol     | String | Implied volatility when filled <br>Only applicable to options; return "" for other instrument types |
| fillPxUsd     | String | Options price when filled, in the unit of USD <br>Only applicable to options; return "" for other instrument types |
| fillMarkVol   | String | Mark volatility when filled <br>Only applicable to options; return "" for other instrument types |
| fillFwdPx     | String | Forward price when filled <br>Only applicable to options; return "" for other instrument types |
| fillMarkPx    | String | Mark price when filled <br>Applicable to `FUTURES`, `SWAP`, `OPTION` |
| side          | String   | Order side,  `buy`  `sell`                              |
| posSide       | String   | Position side <br>`long`  `short`   <br>it returns `net` in`net` mode. |
| execType      | String   | Liquidity taker or maker<br>`T`: taker<br>`M`: maker<br>Not applicable to system orders such as ADL and liquidation |
| feeCcy        | String   | Trading fee or rebate currency                                               |
| fee           | String   | The amount of trading fee or rebate. The trading fee deduction is negative, such as '-0.01'; the rebate is positive, such as '0.01'.                                               |
| ts            | String   | Data generation time,  Unix timestamp format in milliseconds, e.g. `1597026383085`. |
| fillTime      | String   | Trade time which is the same as `fillTime` for the order channel. |
| feeRate      | String   | Fee rate. This field is returned for `SPOT` and `MARGIN` only                                                                                                                             |
