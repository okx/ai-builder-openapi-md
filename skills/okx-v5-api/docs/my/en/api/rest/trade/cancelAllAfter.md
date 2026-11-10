---
site: my
lang: en
name: POST / Cancel All After
method: POST
path: /api/v5/trade/cancel-all-after
module: trade
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### POST / Cancel All After

Cancel all pending orders after the countdown timeout. Applicable to all trading symbols through order book (except Spread trading)<br>

#### Rate Limit: 1 request per second
#### Rate limit rule: User ID + tag

#### HTTP Request

`POST /api/v5/trade/cancel-all-after`

> Request Example

```shell
POST /api/v5/trade/cancel-all-after
{
   "timeOut":"60"
}
```

```python
import okx.Trade as Trade

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading: 0, Demo trading: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# Set cancel all after
result = tradeAPI.cancel_all_after(
    timeOut="10"
)

print(result)

```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                |
| :-------- | :----- |:---------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| timeOut  | String | Yes       | The countdown for order cancellation, with second as the unit.<br>Range of value can be 0, [10, 120]. <br>Setting timeOut to 0 disables Cancel All After. |
| tag | String | No | CAA order tag <br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 16 characters. |

> Response Example

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "tag":"",
            "ts":"1587971400"
        }
    ]
}
```

#### Response Parameters

| Parameter | Type   | Description                           |
| :-------- | :----- |:--------------------------------------|
| triggerTime     | String | The time the cancellation is triggered.<br>triggerTime=0 means Cancel All After is disabled. |
| tag     | String | CAA order tag |
| ts     | String | The time the request is received. |

<aside class="notice">
Users are recommended to send heartbeat to the exchange every second. When the cancel all after is triggered, the trading engine will cancel orders on behalf of the client one by one and this operation may take up to a few seconds. This feature is intended as a protection mechanism for clients only and clients should not use this feature as part of their trading strategies.
</aside>

<aside class="notice">
<br>
To use tag level CAA, first, users need to set tags for their orders using the `tag` request parameter in the placing orders endpoint. When calling the CAA endpoint, if the `tag` request parameter is not provided, the default will be to set CAA at the account level. In this case, all pending orders for all order book trading symbols under that sub-account will be cancelled when CAA triggers, consistent with the existing logic. If the `tag` request parameter is provided, CAA will be set at the order tag level. When triggered, only pending orders of order book trading symbols with the specified tag will be canceled, while orders with other tags or no tags will remain unaffected.
<br>
<br>
Users can run a maximum of 20 tag level CAAs simultaneously under the same sub-account. The system will only count live tag level CAAs. CAAs that have been triggered or revoked by the user will not be counted. The user will receive error code 51071 when exceeding the limit.
</aside>
