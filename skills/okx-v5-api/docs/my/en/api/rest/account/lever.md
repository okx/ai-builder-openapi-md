---
site: my
lang: en
name: Set leverage
method: POST
path: /api/v5/account/set-leverage
module: account
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### Set leverage

 <br>There are 10 different scenarios for leverage setting: <br><br>
1. Set leverage for `MARGIN` instruments under `isolated-margin` trade mode at pairs level. <br>
2. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Spot mode (enabled borrow) at currency level. <br>
3. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Futures mode account mode at pairs level. <br>
4. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Multi-currency margin at currency level. <br>
5. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Portfolio margin at currency level. <br>
6. Set leverage for `FUTURES` instruments under `cross-margin` trade mode at underlying level. <br>
7. Set leverage for `FUTURES` instruments under `isolated-margin` trade mode and buy/sell position mode at contract level. <br>
8. Set leverage for `FUTURES` instruments under `isolated-margin` trade mode and long/short position mode at contract and position side level. <br>
9. Set leverage for `SWAP` instruments under `cross-margin` trade at contract level. <br>
10. Set leverage for `SWAP` instruments under `isolated-margin` trade mode and buy/sell position mode at contract level. <br>
11. Set leverage for `SWAP` instruments under `isolated-margin` trade mode and long/short position mode at contract and position side level. <br><br>

Note that the request parameter `posSide` is only required when margin mode is isolated in long/short position mode for FUTURES/SWAP instruments (see scenario 8 and 11 above). <br>
Please refer to the request examples on the right for each case. <br>

#### Rate limit: 20 requests per 2 seconds
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/set-leverage`

> Request Example

```shell
# 1. Set leverage for `MARGIN` instruments under `isolated-margin` trade mode at pairs level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT",
    "lever":"5",
    "mgnMode":"isolated"
}

# 2. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Spot mode (enabled borrow) at currency level.
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 3. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Futures mode account mode at pairs level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT",
    "lever":"5",
    "mgnMode":"cross"
}

# 4. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Multi-currency margin at currency level.
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 5. Set leverage for `MARGIN` instruments under `cross-margin` trade mode and Portfolio margin at currency level.
POST /api/v5/account/set-leverage
body
{
    "ccy":"BTC",
    "lever":"5",
    "mgnMode":"cross"
}

# 6. Set leverage for `FUTURES` instruments under `cross-margin` trade mode at underlying level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "mgnMode":"cross"
}

# 7. Set leverage for `FUTURES` instruments under `isolated-margin` trade mode and buy/sell order placement mode at contract level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "mgnMode":"isolated"
}

# 8. Set leverage for `FUTURES` instruments under `isolated-margin` trade mode and long/short order placement mode at contract and position side level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-200802",
    "lever":"5",
    "posSide":"long",
    "mgnMode":"isolated"
}

# 9. Set leverage for `SWAP` instruments under `cross-margin` trade at contract level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "mgnMode":"cross"
}

# 10. Set leverage for `SWAP` instruments under `isolated-margin` trade mode and buy/sell order placement mode at contract level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "mgnMode":"isolated"
}

# 11. Set leverage for `SWAP` instruments under `isolated-margin` trade mode and long/short order placement mode at contract and position side level.
POST /api/v5/account/set-leverage
body
{
    "instId":"BTC-USDT-SWAP",
    "lever":"5",
    "posSide":"long",
    "mgnMode":"isolated"
}
```

```python
import okx.Account as Account

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # Production trading:0 , demo trading:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# Set leverage for MARGIN instruments under isolated-margin trade mode at pairs level.
result = accountAPI.set_leverage(
    instId="BTC-USDT",
    lever="5",
    mgnMode="isolated"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required | Description                                                                                                                                                                                                                                                       |
| :-------- | :----- | :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instId    | String | Conditional | Instrument ID<br> Only applicable to `cross` `FUTURES` `SWAP` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`, `cross` `MARGIN``FUTURES``SWAP` and `isolated` position.<br> And required in applicable scenarios.                                                                                                                                  |
| ccy       | String | Conditional | Currency used for margin, used for the leverage setting for the currency in auto borrow.<br>Only applicable to `cross` `MARGIN` of `Spot mode`/`Multi-currency margin`/`Portfolio margin`.<br> And required in applicable scenarios.  |
| lever     | String | Yes      | Leverage                                                                                                                                                                                                                                                          |
| mgnMode   | String | Yes      | Margin mode<br>`isolated` `cross` <br>Can only be `cross` if `ccy` is passed.                                                                                                                                                                                     |
| posSide   | String | Conditional | Position side<br>`long` `short`<br>Only required when margin mode is `isolated` in `long/short` mode for `FUTURES`/`SWAP`.                                                                                                                                        |

> Response Example

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
      "lever": "30",
      "mgnMode": "isolated",
      "instId": "BTC-USDT-SWAP",
      "posSide": "long"
    }
  ]
}
```

#### Response Parameters

| Parameter | Type   | Description                       |
| :-------- | :----- | :-------------------------------- |
| lever     | String | Leverage                          |
| mgnMode   | String | Margin mode<br>`cross` `isolated` |
| instId    | String | Instrument ID                     |
| posSide   | String | Position side                     |

<aside class="notice">
When setting leverage for `cross` `FUTURES`/`SWAP` at the underlying level, pass in any instId and mgnMode(`cross`).
</aside>

<aside class="notice">
Leverage cannot be adjusted for the cross positions of Expiry Futures and Perpetual Futures under the portfolio margin account.
</aside>
