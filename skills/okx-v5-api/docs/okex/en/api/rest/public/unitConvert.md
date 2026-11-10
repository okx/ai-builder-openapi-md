---
site: okex
lang: en
name: Unit convert
method: GET
path: /api/v5/public/convert-contract-coin
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Unit convert

Convert the crypto value to the number of contracts, or vice versa

#### Rate Limit: 10 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP Request

`GET /api/v5/public/convert-contract-coin`

> Request Example

```shell
GET /api/v5/public/convert-contract-coin?instId=BTC-USD-SWAP&px=35000&sz=0.888
```

```python
import okx.PublicData as PublicData

flag = "0"  # Production trading: 0, Demo trading: 1

publicDataAPI = PublicData.PublicAPI(flag=flag)

# Convert the crypto value to the number of contracts, or vice versa
result = publicDataAPI.get_convert_contract_coin(
    instId="BTC-USD-SWAP",
    px="35000",
    sz="0.888"
)
print(result)
```

#### Request Parameters

| Parameter | Type   | Required    | Description                                                                                                                                                                                                                                                               |
|:----------| :----- |:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type      | String | No          | Convert type<br>`1`: Convert currency to contract<br>`2`: Convert contract to currency<br>The default is `1`                                                                                                                                                              |
| instId    | String | Yes         | Instrument ID<br>only applicable to `FUTURES`/`SWAP`/`OPTION`                                                                                                                                                                                                             |
| sz        | String | Yes         | Quantity to buy or sell<br>It is quantity of currency while converting currency to contract; <br>It is quantity of contract while converting contract to currency.                                                                                                        |
| px        | String | Conditional | Order price<br>For crypto-margined contracts, it is necessary while converting.<br>For USDT-margined contracts, it is necessary while converting between usdt and contract.<br>It is optional while converting between coin and contract. <br>For OPTION, it is optional. |
| unit      | String | No          | The unit of currency<br>`coin`<br>`usds`: USDT/USDC<br>The default is `coin`, only applicable to USDⓈ-margined contracts from `FUTURES`/`SWAP`                                                                                                                            |
| opType    | String | No          | Order type<br>`open`: round down sz when opening positions <br>`close`: round sz to the nearest when closing positions <br>The default is `close` <br>Applicable to `FUTURES` `SWAP`                                                                                      | 

> Response Example

```json
{
    "code": "0",
    "data": [
        {
            "instId": "BTC-USD-SWAP",
            "px": "35000",
            "sz": "311",
            "type": "1",
            "unit": "coin"
        }
    ],
    "msg": ""
}
```

#### Response Parameters

| **Parameter** | **Type** | **Description**                                                                                                                                       |
|:--------------| :------- |:------------------------------------------------------------------------------------------------------------------------------------------------------|
| type          | String   | Convert type <br>`1`: Convert currency to contract<br>`2`: Convert contract to currency                                                                |
| instId        | String   | Instrument ID                                                                                                                                         |
| px            | String   | Order price                                                                                                                                           |
| sz            | String   | Quantity to buy or sell<br>It is quantity of contract while converting currency to contract<br>It is quantity of currency while contract to currency. |
| unit          | String   | The unit of currency<br>`coin`<br>`usds`: USDT/USDC                                                                                                   |
