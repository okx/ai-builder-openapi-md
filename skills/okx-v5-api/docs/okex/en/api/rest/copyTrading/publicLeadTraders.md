---
site: okex
lang: en
name: GET / Lead trader ranks
method: GET
path: /api/v5/copytrading/public-lead-traders
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / Lead trader ranks 

Public endpoint. Retrieve lead trader ranks.

#### Rate limit: 5 requests per 2 seconds
#### Rate limit rule: IP

#### HTTP request

`GET /api/v5/copytrading/public-lead-traders`

> Request example

```shell
GET /api/v5/copytrading/public-lead-traders?instType=SWAP

```

#### Request Parameters

| Parameter  | Type   | Required | Description                                                                                                                                                                                                                                                                                            |
| :------ | :----- |:---------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| instType     | String | No       | Instrument type<br>`SWAP`, the default value                                                                                                                                                                                                                                                          |
| sortType    | String | No       | Sort type<br>`overview`: overview, the default value<br>`pnl`: profit and loss<br>`aum`: assets under management<br>`win_ratio`: win ratio<br>`pnl_ratio`: pnl ratio<br>`current_copy_trader_pnl`: current copy trader pnl                                                                   |
| state       | String | No        | Lead trader state<br>`0`: All lead traders, the default, including vacancy and non-vacancy <br>`1`: lead traders who have vacancy                                                                                                                                                                    |
| minLeadDays | String | No        | Minimum lead days<br>`1`: 7 days<br>`2`: 30 days<br>`3`: 90 days<br>`4`: 180 days                                                                                                                                                                                                                 |
| minAssets   | String | No        | Minimum assets in USDT                                                                                                                                                                                                                                                                                 |
| maxAssets   | String | No        | Maximum assets in USDT                                                                                                                                                                                                                                                                                 |
| minAum      | String | No        | Minimum assets in USDT under management.                                                                                                                                                                                                                                                               |
| maxAum      | String | No        | Maximum assets in USDT under management.                                                                                                                                                                                                                                                               |
| dataVer     | String | No        | Data version. It is 14 numbers. e.g. 20231010182400. Generally, it is used for pagination <br>A new version will be generated every 10 minutes. Only last 5 versions are stored<br>The default is latest version. If it is not exist, error will not be throwed and the latest version will be used. |
| page        | String | No        | Page for pagination                                                                                                                                                                                                                                                                                    |
| limit       | String | No        | Number of results per request. The maximum is 20; the default is 10                                                                                                                                                                                                                                    |

> Response example

```json
{
    "code": "0",
    "data": [
        {
            "dataVer": "20231129213200",
            "ranks": [
                {
                    "accCopyTraderNum": "3536",
                    "aum": "1509265.3238761567721365",
                    "ccy": "USDT",
                    "copyState": "0",
                    "copyTraderNum": "999",
                    "leadDays": "156",
                    "maxCopyTraderNum": "1000",
                    "nickName": "Crypto to the moon",
                    "pnl": "48805.1105999999972258",
                    "pnlRatio": "1.6898",
                    "pnlRatios": [
                        {
                            "beginTs": "1701187200000",
                            "pnlRatio": "1.6744"
                        },
                        {
                            "beginTs": "1700755200000",
                            "pnlRatio": "1.649"
                        }
                    ],
                    "portLink": "https://static.okx.com/cdn/okex/users/headimages/20230624/f49a683aaf5949ea88b01bbc771fb9fc",
                    "traderInsts": [
                        "ICP-USDT-SWAP",
                        "MINA-USDT-SWAP"
                        
                    ],
                    "uniqueCode": "540D011FDACCB47A",
                    "winRatio": "0.6957"
                }
            ],
            "totalPage": "1"
        }
    ],
    "msg": ""
}
```

#### Response parameters

| **Parameter**      | **Type** | **Description**                                 |
|:-------------------| :------- |:------------------------------------------------|
| dataVer            | String | Data version                                    |
| totalPage          | String | Total number of pages                           |
| ranks              | Array of objects  | The rank information of lead traders            |
| > aum              | String | assets under management                         |
| > copyState        | String     | Current copy state <br>`0`: non-copy, `1`: copy                |
| > maxCopyTraderNum | String | Maximum number of copy traders                  |
| > copyTraderNum    | String | Current number of copy traders                  |
| > accCopyTraderNum | String | Accumulated number of copy traders              |
| > portLink         | String | Portrait link                                   |
| > nickName         | String | Nick name                                       |
| > ccy              | String | Margin currency                                 |
| > uniqueCode       | String | Lead trader unique code                         |
| > winRatio         | String | Win ratio, 0.1 represents 10%                   |
| > leadDays         | String | Lead days                                       |
| > traderInsts      | Array of strings  | Contract list which lead trader is leading      |
| > pnl              | String | Pnl (in USDT) of last 90 days. |
| > pnlRatio         | String | Pnl ratio of last 90 days. 0.1 represents 10%   |
| > pnlRatios        | Array of objects  | Pnl ratios                                      |
| >> beginTs         | String  | Begin time of pnl ratio on that day             |
| >> pnlRatio        | String  | Pnl ratio on that day                           |
