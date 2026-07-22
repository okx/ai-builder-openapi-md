---
site: okex
lang: zh
name: 仓位创建器趋势图
method: POST
path: /api/v5/account/position-builder-graph
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 仓位创建器趋势图 [trading-account-rest-api-position-builder-trend-graph]

#### 限速：1次/5s

#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/account/position-builder-graph`

> 请求示例

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

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| inclRealPosAndEq   | Boolean         | 否    | 是否代入已有仓位和资产<br>默认为`true`                                           |
| simPos             | Array of objects | 否    | 模拟仓位列表                                                             |
| > instId           | String          | 是    | 交易产品ID，如 `BTC-USDT-SWAP`<br>适用于 `SWAP`/`FUTURES`/`OPTION`          |
| > pos              | String          | 是    | 持仓量                                                                |
| > avgPx            | String          | 是    | 平均开仓价格                                                             |
| > lever            | String          | 否    | 杠杆<br>仅适用于在跨币种保证金模式下指定交易产品的杠杆。如果用户不传，则选择默认杠杆为`1`。                  |
| simAsset         | Array of objects | 否    | 模拟资产<br>当`inclRealPosAndEq`为`true`，只考虑真实资产，会忽略虚拟资产                 |
| > ccy            | String          | 是    | 币种，如 `BTC`                                                         |
| > amt            | String          | 是    | 币种数量<br>可以为负，代表减少币种资产                                              |
| type | String | 是 | 趋势图类型<br> `mmr` |
| mmrConfig | Object | 是 | MMR配置 |
| > acctLv | String | 否 | 切换至账户模式<br>`3`：跨币种保证金模式<br>`4`：组合保证金模式   |
| > lever | String | 否 | 跨币种下整体的全仓合约杠杆数量，默认为`1`。<br>如果超过允许的杠杆倍数，按照最大的杠杆设置。<br>适用于`跨币种保证金模式` |

> 返回示例

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

#### 返回参数

| **参数名** | **类型** | **描述** |
| --- | --- | --- |
| type | String | 趋势图类型<br> `mmr` |
| mmrData | Array | MMR数据<br> 以shockFactor升序返回 |
| > shockFactor | String | 价格变动比例，数据范围 -1 到 1. |
| > mmr | String | 维持保证金 |
| > mmrRatio | String | 维持保证金率 |
