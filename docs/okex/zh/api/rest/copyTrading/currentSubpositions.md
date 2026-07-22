---
site: okex
lang: zh
name: GET / 获取当前带单
method: GET
path: /api/v5/copytrading/current-subpositions
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取当前带单 [order-book-trading-copy-trading-get-existing-lead-positions]

获取当前未平仓的带单仓位。<br>

按照开仓时间倒序排列。

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/current-subpositions`

> 请求示例

```shell
GET /api/v5/copytrading/current-subpositions?instId=BTC-USDT-SWAP

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                       |
| :------ | :----- |:-----|:-------------------------------------------------------------------------|
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约<br>默认返回所有业务线的信息                             |
| instId  | String | 否    | 产品ID ，如`BTC-USDT-SWAP`                                                   |
| after    | String | 否       | 请求此id之前（更旧的数据）的分页内容，传的值为对应接口的`subPosId`                                  |
| before   | String | 否       | 请求此id之后（更新的数据）的分页内容，传的值为对应接口的`subPosId`                                  |
| limit    | String | 否       | 分页返回的结果集数量，最大为500，不填默认返回500条                                             |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "algoId": "",
            "ccy": "USDT",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "12.6417",
            "markPx": "38205.8",
            "mgnMode": "isolated",
            "openAvgPx": "37925.1",
            "openOrdId": "",
            "openTime": "1701231120479",
            "posSide": "net",
            "slOrdPx": "",
            "slTriggerPx": "",
            "subPos": "1",
            "subPosId": "649945658862370816",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "uniqueCode": "25CD5A80241D6FE6",
            "upl": "0.2807",
            "uplRatio": "0.0222042921442527",
            "availSubPos": "1"
        },
        {
            "algoId": "",
            "ccy": "USDT",
            "instId": "BTC-USDT-SWAP",
            "instType": "SWAP",
            "lever": "3",
            "margin": "12.6263333333333333",
            "markPx": "38205.8",
            "mgnMode": "isolated",
            "openAvgPx": "37879",
            "openOrdId": "",
            "openTime": "1701225074786",
            "posSide": "net",
            "slOrdPx": "",
            "slTriggerPx": "",
            "subPos": "1",
            "subPosId": "649920301388038144",
            "tpOrdPx": "",
            "tpTriggerPx": "",
            "uniqueCode": "25CD5A80241D6FE6",
            "upl": "0.3268",
            "uplRatio": "0.0258824150584758",
            "availSubPos": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                                                                            |
| :---------- | :------- |:----------------------------------------------------------------------------------|
| instId      | String   | 产品ID                                                                              |
| subPosId      | String   | 带单仓位ID                                                                        |
| posSide         | String   | 持仓方向<br>long：开平仓模式开多 <br>short：开平仓模式开空 <br>net：买卖模式（subPos为正代表开多，subPos为负代表开空） |
| mgnMode       | String   | 保证金模式，`isolated`：逐仓 ；`cross`：全仓                                                   |
| lever     | String   | 杠杆倍数                                                                              |
| openOrdId         | String   | 交易员开仓订单号，仅适用于带单仓位                                                                 |
| openAvgPx          | String   | 开仓均价                                                                              |
| openTime          | String   | 开仓时间                                                                              |
| subPos         | String   | 持仓张数                                                                              |
| tpTriggerPx     | String   | 止盈触发价                                                                  |
| slTriggerPx        | String   | 止损触发价                                                                  |
| algoId     | String   | 止盈止损委托单ID                                                                         |
| instType  | String     | 产品类型<br>SPOT：币币<br>SWAP：永续合约                                                    |
| tpOrdPx     | String    | 止盈委托价，市价时为-1                                                                      |
| slOrdPx     | String    | 止损委托价，市价时为-1                                                                      |
| margin     | String     | 保证金                                                                               |
| upl     | String     | 未实现收益                                                                             |
| uplRatio     | String      | 未实现收益率                                                                            |
| markPx     | String       | 最新标记价格，仅适用于合约                                                                     |
| uniqueCode     | String       | 交易员唯一标识代码                                                                         |
| ccy     | String       | 保证金币种                                                                             |
| availSubPos     | String    | 可平张数/币数                                                                           |
