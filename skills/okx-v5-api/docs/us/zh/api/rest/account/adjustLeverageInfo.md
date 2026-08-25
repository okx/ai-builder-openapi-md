---
site: us
lang: zh
name: 获取杠杆倍数预估信息
method: GET
path: /api/v5/account/adjust-leverage-info
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com:8443
supported: true
---

### 获取杠杆倍数预估信息 [trading-account-rest-api-get-leverage-estimated-info]

获取指定杠杆倍数下，相关的预估信息。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/adjust-leverage-info`

> 请求示例

```shell
GET /api/v5/account/adjust-leverage-info?instType=MARGIN&mgnMode=isolated&lever=3&instId=BTC-USDT

```

#### 请求参数

| 参数名       | 类型   | 是否必须 | 描述                                                                                 |
|:----------| :----- |:-----|:-----------------------------------------------------------------------------------|
| instType  | String | 是    | 产品类型<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约                             |
| mgnMode   | String | 是    | 保证金模式<br>`isolated`：逐仓<br>`cross`：全仓                                               |
| lever     | String | 是    | 杠杆倍数                                                                               |
| instId    | String | 可选   | 产品ID，如 `BTC-USDT`<br>必填的场景有：交割永续，逐仓杠杆，以及`合约模式`下全仓杠杆。                               |
| ccy       | String | 可选   | 保证金币种，如 `BTC`<br>逐仓杠杆及`合约模式`/`跨币种保证金模式`/`组合保证金模式`的全仓杠杆时必填。                      |
| posSide   | String | 否    | 持仓方向<br>`net`: 默认值，代表买卖模式<br>`long`: 开平模式下的多仓<br>`short`：开平模式下的空仓<br>适用于`交割`/`永续`。 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "estAvailQuoteTrans": "",
            "estAvailTrans": "1.1398040558348279",
            "estLiqPx": "",
            "estMaxAmt": "10.6095865868904898",
            "estMgn": "0.0701959441651721",
            "estQuoteMaxAmt": "176889.6871254563042714",
            "estQuoteMgn": "",
            "existOrd": false,
            "maxLever": "10",
            "minLever": "0.01"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                            |
| :--------- | :------- |:------------------------------------------------------------------|
| estAvailQuoteTrans     | String   | 对应杠杆倍数下，计价货币预估可转出的保证金数量<br>全仓时，为交易账户最大可转出<br>逐仓时，为逐仓仓位可减少的保证金。<br> 仅适用于`杠杆`   |
| estAvailTrans    | String   | 对应杠杆倍数下，预估可转出的保证金数量<br>全仓时，为交易账户最大可转出<br>逐仓时，为逐仓仓位可减少的保证金<br>对于`杠杆`，单位为交易货币<br>不适用于`交割`, `永续`的逐仓，调大杠杆的场景  |
| estLiqPx    | String   | 对应杠杆倍数下的预估强平价，仅在有仓位时有值                                            |
| estMgn      | String   | 对应杠杆倍数下，仓位预估所需的保证金数量<br>对于杠杆仓位，为所需交易货币保证金<br>对于交割或永续仓位，为仓位所需保证金 |
| estQuoteMgn      | String   | 对应杠杆倍数下，仓位预估所需的计价货币保证金数量                                          |
| estMaxAmt      | String   | 对于杠杆，为对应杠杆倍数下，交易货币预估最大可借<br>对于交割和永续，为对应杠杆倍数下，预估的最大可开张数           |
| estQuoteMaxAmt      | String   | 对应杠杆倍数下，杠杆计价货币预估最大可借                                              |
| existOrd      | Boolean   | 当前是否存在挂单 <br>`true`：存在挂单<br>`false`：不存在挂单                       |
| maxLever      | String   | 最大杠杆倍数                                                            |
| minLever      | String   | 最小杠杆倍数                                                            |
