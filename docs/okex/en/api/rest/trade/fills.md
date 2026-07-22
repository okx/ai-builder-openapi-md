---
site: okex
lang: en
name: GET / Transaction details (last 3 days)
method: GET
path: /api/v5/trade/fills
module: trade
rest: https://www.okx.com
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

| Parameter  | Type     | Required | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|:-----------|:---------|:---------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType   | String   | No       | Instrument type<br>`SPOT`<br>`MARGIN`<br>`SWAP`<br>`FUTURES`<br>`OPTION`<br>`EVENTS`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| instFamily | String   | No       | Instrument family<br>Applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                      
| instId     | String   | No       | Instrument ID, e.g. `BTC-USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ordId      | String   | No       | Order ID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| subType    | String   | No       | Transaction type <br>`1`: Buy<br>`2`: Sell<br>`3`: Open long<br>`4`: Open short<br>`5`: Close long<br>`6`: Close short <br>`100`: Partial liquidation close long<br>`101`: Partial liquidation close short<br>`102`: Partial liquidation buy<br>`103`: Partial liquidation sell<br>`104`: Liquidation long<br>`105`: Liquidation short<br>`106`: Liquidation buy <br>`107`: Liquidation sell <br>`110`: Liquidation transfer in<br>`111`: Liquidation transfer out <br>`118`: System token conversion transfer in<br>`119`: System token conversion transfer out<br> `112`: Delivery long<br> `113`: Delivery short  <br>`125`: ADL close long<br>`126`: ADL close short<br>`127`: ADL buy<br>`128`: ADL sell <br>`212`: Auto borrow of quick margin<br>`213`: Auto repay of quick margin <br>`204`: block trade buy<br>`205`: block trade sell<br>`206`: block trade open long<br>`207`: block trade open short<br>`208`: block trade close long<br>`209`: block trade close short<br>`236`: Easy convert in<br>`237`: Easy convert out<br>`270`: Spread trading buy<br>`271`: Spread trading sell<br>`272`: Spread trading open long<br>`273`: Spread trading open short<br>`274`: Spread trading close long<br>`275`: Spread trading close short<br> `324`: Move position buy<br> `325`: Move position sell<br> `326`: Move position open long<br> `327`: Move position open short<br> `328`: Move position close long<br> `329`: Move position close short <br>`376`: Collateralized borrowing auto conversion buy<br>`377`: Collateralized borrowing auto conversion sell<br>`410`: Buy yes<br>`411`: Buy no<br>`412`: Sell yes<br>`413`: Sell no<br>`414`: Yes expiry<br>`415`: No expiry|
| after      | String   | No       | Pagination of data to return records earlier than the requested `billId`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| before     | String   | No       | Pagination of data to return records newer than the requested `billId`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| begin      | String   | No       | Filter with a begin timestamp  `ts`. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| end        | String   | No       | Filter with an end timestamp  `ts`. Unix timestamp format in milliseconds, e.g. `1597026383085`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| limit      | String   | No       | Number of results per request. The maximum is `100`; The default is `100`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

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
            "subType": "1",
            "fillMarkVol": "",
            "tag": "",
            "fillTime": "1708587373361",
            "execType": "T",
            "fillIdxPx": "",
            "tradeId": "744876980",
            "fillMarkPx": "",
            "feeCcy": "BTC",
            "ts": "1708587373362",
            "tradeQuoteCcy": "USDT"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                                                                |
|:--------------| :------- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType      | String   | Instrument type                                                                                                                                                                                |
| instId        | String   | Instrument ID                                                                                                                                                                                  |
| tradeId       | String   | Last trade ID                                                                                                                                                                                  |
| ordId         | String   | Order ID                                                                                                                                                                                       |
| clOrdId       | String   | Client Order ID as assigned by the client                                                                                                                                                      |
| billId        | String   | Bill ID                                                                                                                                                                                        |
| subType    | String  | Transaction type                                                                                                 |
| tag           | String   | Order tag                                                                                                                                                                                      |
| fillPx        | String   | Last filled price. It is the same as the px from "Get bills details".                                                                                                                          |
| fillSz        | String   | Last filled quantity                                                                                                                                                                           |
| fillIdxPx     | String   | Index price at the moment of trade execution <br>For cross currency spot pairs, it returns baseCcy-USDT index price. For example, for LTC-ETH, this field returns the index price of LTC-USDT. |
| fillPnl       | String   | Realised P&L from this fill for close-position trades, denominated in the settlement currency (see `feeCcy`). Positive = realised gain; negative = realised loss. Formula: (fillPx − avgPx) × fillSz × ctVal (linear) or (1/avgPx − 1/fillPx) × fillSz × ctVal (inverse). Returns 0 for opening trades.                                                             |
| fillPxVol     | String | Implied volatility when filled <br>Only applicable to options; return "" for other instrument types                                                                                            |
| fillPxUsd     | String | Options price when filled, in the unit of USD <br>Only applicable to options; return "" for other instrument types                                                                             |
| fillMarkVol   | String | Mark volatility when filled <br>Only applicable to options; return "" for other instrument types                                                                                               |
| fillFwdPx     | String | Forward price when filled <br>Only applicable to options; return "" for other instrument types                                                                                                 |
| fillMarkPx    | String | Mark price when filled <br>Applicable to `FUTURES`, `SWAP`, `OPTION`                                                                                                                           |
| side          | String   | Order side,  `buy`  `sell`                                                                                                                                                                     |
| posSide       | String   | Position side <br>`long`  `short`   <br>it returns `net` in`net` mode.                                                                                                                         |
| execType      | String   | Liquidity taker or maker<br>`T`: taker<br>`M`: maker<br>Not applicable to system orders such as ADL and liquidation                                                                            |
| feeCcy        | String   | Trading fee or rebate currency                                                                                                                                                                 |
| fee           | String   | The amount of trading fee or rebate. The trading fee deduction is negative, such as '-0.01'; the rebate is positive, such as '0.01'.                                                           |
| ts            | String   | Timestamp when this fill record was generated by the system, in Unix milliseconds (UTC). Note: this differs from `fillTime`, which is the actual trade execution time. For chronological ordering of trades, sort by `fillTime`, not `ts`. |
| fillTime      | String   | Trade time which is the same as `fillTime` for the order channel.                                                                                                                              |
| feeRate      | String   | Fee rate. This field is returned for `SPOT` and `MARGIN` only                                                                                                                             |
| tradeQuoteCcy | String | The quote currency for trading. |

<aside class="notice">
tradeId<br>
For partial_liquidation, full_liquidation, or adl, when it comes to fill information, this field will be assigned a negative value to distinguish it from other matching transaction scenarios, when it comes to order information, this field will be 0.
</aside>

<aside class="notice">
ordId<br>
Order ID, always "" for block trading.<br>
</aside>

<aside class="notice">
clOrdId<br>
Client-supplied order ID, always "" for block trading.
</aside>
