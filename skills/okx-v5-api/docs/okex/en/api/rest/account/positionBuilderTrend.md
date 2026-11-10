---
site: okex
lang: en
name: Position builder trend graph
method: POST
path: /api/v5/account/position-builder-graph
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### Position builder trend graph

#### Rate limit: 1 request per 5 seconds

#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/account/position-builder-graph`

> Request Example

```shell
{
   "inclRealPosAndEq":false,
   "simPos":[
      {
         "pos":"-10",
         "instId":"BTC-USDT-SWAP",
         "avgPx":"100000"
      },
      {
         "pos":"10",
         "instId":"LTC-USDT-SWAP",
         "avgPx":"8000"
      }
   ],
   "simAsset":[
      {
         "ccy":"USDT",
         "amt":"100"
      }
   ],
   "greeksType":"CASH",
   "type":"mmr",
   "mmrConfig":{
      "acctLv":"3",
      "lever":"1"
   }
}
```

#### Request Parameters

| **Parameter** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| inclRealPosAndEq | Boolean | No | Whether to import existing positions and assets<br> The default is `true` |
| simPos | Array of objects | No | List of simulated positions |
| > instId | String | Yes | Instrument ID, e.g. `BTC-USDT-SWAP`<br> Applicable to `SWAP`/`FUTURES`/`OPTION` |
| > pos | String | Yes | Quantity of positions |
| > avgPx | String | Yes | Average open price |
| > lever | String | No | leverage<br> Only applicable to `Multi-currency margin`<br> The default is `1`<br> If the allowed leverage is exceeded, set according to the maximum leverage. |
| simAsset | Array of objects | No | List of simulated assets<br> When `inclRealPosAndEq` is `true`, only real assets are considered and virtual assets are ignored |
| > ccy | String | Yes | Currency, e.g. `BTC` |
| > amt | String | Yes | Currency amount |
| type | String | Yes | Trending graph type<br> `mmr` |
| mmrConfig | Object | Yes | MMR configuration |
| > acctLv | String | No | Switch to account mode<br> `3`: Multi-currency margin<br> `4`: Portfolio margin |
| > lever | String | No | Cross margin leverage in Multi-currency margin mode, the default is `1`.<br> If the allowed leverage is exceeded, set according to the maximum leverage.<br> Only applicable to `Multi-currency margin` |

> Response Example

```json
{
    "code": "0",
    "data": [
         {
            "type": "mmr",
            "mmrData": [
               ......
               {
                     "mmr": "1415.0254039225917",
                     "mmrRatio": "-47.45603627655477",
                     "shockFactor": "-0.94"
               },
               {
                     "mmr": "1417.732491243024",
                     "mmrRatio": "-47.436684685735386",
                     "shockFactor": "-0.93"
               }
               ......
            ]
         }
    ],
    "msg": ""
}

```

#### Response Parameters

| **Parameters** | **Types** | **Description** |
| --- | --- | --- |
| type | String | Graph type<br> `mmr` |
| mmrData | Array | Array of mmrData<br> Return data in shockFactor ascending order |
| > shockFactor | String | Price change ratio, data range -1 to 1. |
| > mmr | String | Mmr at specific price |
| > mmrRatio | String | Maintenance margin ratio at specific price |
