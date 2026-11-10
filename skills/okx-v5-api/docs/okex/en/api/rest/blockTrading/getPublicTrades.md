---
site: okex
lang: en
name: Get public multi-leg transactions of block trades
method: GET
path: /api/v5/rfq/public-trades
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Get public multi-leg transactions of block trades

Retrieves the executed block trades. The data will be updated 15 minutes after the block trade execution.

#### Rate Limit: 5 requests per 2 seconds
#### Rate limit rule: IP
#### HTTP Request

`GET /api/v5/rfq/public-trades`

> Request Example

```shell
GET /api/v5/rfq/public-trades
```

```python
import okx.BlockTrading as BlockTrading

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# Retrieves the executed block trades
result = blockTradingAPI.get_public_trades()
print(result)
```

#### Request parameters
| Parameter  | Type | Required | Description                                              |
| :------ | :----- | :----- | :-----------------------------------------------------------  |
| beginId |String |No |The starting blockTdId the request to begin with. Pagination of data to return records newer than the requested `blockTdId`, not including beginId.                |
| endId	  |String |No |The last blockTdId the request to end with. Pagination of data to return records earlier than the requested `blockTdId`, not including endId.                    |
| limit	  |String |No |Number of results per request. The maximum is 100 which is also the default value.                                                                    |

> Response Example

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "blockTdId": "439161457415012352",
            "groupId": "",
            "legs": [
                {
                    "instId": "BTC-USD-210826",
                    "side": "sell",
                    "sz": "100",
                    "px": "11000",
                    "tradeId": "439161457415012354"
                },
                {
                    "instId": "BTC-USD-SWAP",
                    "side": "sell",
                    "sz": "100",
                    "px": "50",
                    "tradeId": "439161457415012355"
                },
                {
                    "instId": "BTC-USDT",
                    "side": "buy",
                    "sz": "0.1", //for public feed, spot "sz" is in baseccy
                    "px": "10.1",
                    "tradeId": "439161457415012356"
                },
                {
                    "instId": "BTC-USD-210326-60000-C",
                    "side": "buy",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012357"
                },
                {
                    "instId": "BTC-USD-220930-5000-P",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012360"
                },
                {
                    "instId": "BTC-USD-220930-10000-C",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012361"
                },
                {
                    "instId": "BTC-USD-220930-10000-P",
                    "side": "sell",
                    "sz": "200",
                    "px": "0.008",
                    "tradeId": "439161457415012362"
                },
                {
                    "instId": "ETH-USD-220624-100100-C",
                    "side": "sell",
                    "sz": "100",
                    "px": "0.008",
                    "tradeId": "439161457415012363"
                }
            ],
            "strategy":"CALL_CALENDAR_SPREAD",
            "cTime": "1650976251241"
        }
    ]
}
```

#### Response Parameters

| Parameter    | Type   | Description                                              |
|:-------------| :----- | :-----------------------------------------------------------  |
| code	        | String|   The result code, `0` means success.                   |
| msg	         | String|   The error message, not empty if the code is not 0.|
| data	        | Array of objects|	Array of objects containing the results of the public block trade.                                                                              |
| > strategy	  | String|	Option strategy, e.g. CALL_CALENDAR_SPREAD|
| > cTime	     | String|	The time the trade was executed. Unix timestamp in milliseconds.|
| > blockTdId	 | String|	Block trade ID.   |
| > groupId | String | Group RFQ ID<br> Only applicable to group RFQ, return "" for normal RFQ |
| > legs	      | Array of objects|	Legs of trade|
| >> instId	   | String|	Instrument ID, e.g. BTC-USDT-SWAP|
| >> px	       | String|	The price the leg executed|
| >> sz        | String   | Trade quantity <br>For spot trading, the unit is base currency<br/>For `FUTURES`/`SWAP`/`OPTION`, the unit is contract.                              |
| >> side	     | String|	The direction of the leg from the Takers perspective. Valid value can be buy or sell.|
| >> tradeId	  | String|	Last traded ID.|

<aside class="notice">
Group RFQ introduction<br><br>

1. Add new response parameter groupId, facilitating clients to map subaccount execution to group RFQ. Only applicable to group RFQ, return "" for normal RFQ.<br>
2. Data return by this endpoint should be at **parent RFQ level** regardless of the subaccounts allocation. blockTdId and tradeId will be empty.<br>
</aside>
