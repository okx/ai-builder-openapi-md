---
site: okex
lang: zh
name: 获取可报价产品
method: GET
path: /api/v5/rfq/maker-instrument-settings
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取可报价产品 [block-trading-rest-api-get-quote-products]

用于maker查询特定的接受询价和报价的产品, 以及数量和价格范围。

#### 限速: 5次/2s
#### 限速规则：User ID
#### HTTP Requests

`GET /api/v5/rfq/maker-instrument-settings`

> 请求示例

```shell
GET /api/v5/rfq/maker-instrument-settings
```

#### 请求参数
无

> 返回示例

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "OPTION",
            "includeAll": true,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "SOL-USD",
                    "maxBlockSz": "100000",
                    "makerPxBand": "15"
                }
            ]
        },
        {
            "instType": "FUTURES",
            "includeAll": false,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "ETH-USDT",
                    "maxBlockSz": "100000",
                    "makerPxBand": "15"
                }
            ]
        },
        {
            "instType:": "SWAP",
            "includeAll": false,
            "data": [
                {
                    "instFamily": "BTC-USD",
                    "maxBlockSz": "10000",
                    "makerPxBand": "5"
                },
                {
                    "instFamily": "ETH-USDT"
                }
            ]
        },
        {
            "instType:": "SPOT",
            "includeAll": false,
            "data": [
                {
                    "instId": "BTC-USDT"
                },
                {
                    "instId": "TRX-USDT"
                }
            ]
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                               |
| :------ | :----- |:-------------------------------------------------|
| code	         | String | 结果代码，`0` 表示成功                   |
| msg	         | String | 错误信息，如果代码不为`0`，则不为空         |
| data	         | Array of objects| 请求返回值，包含请求结果         |
| > instType       | String | 产品类别，枚举值包括`FUTURES`,`OPTION`,`SWAP`和`SPOT`                                                                                       |
| > includeAll     | Boolean| 是否接收该instType下所有产品。有效值为`true`或`false`。默认`false`。                                                                                 |
| > data         | Array of objects | instType的元素                                                                                                                      |
| >> instFamily  | String | 交易品种<br>`交割`/`永续`/`期权`情况下必填                                                                                                          |
| >> instId      | String | 产品ID，如 `BTC-USDT`。对`SPOT`产品类别有效且必须。                                                                                              |
| >> maxBlockSz  | String | 该种产品最大可交易数量。FUTURES, OPTION and SWAP 的单位是合约数量。SPOT的单位是交易货币。                                                                      | 
| >> makerPxBand | String | 价格限制以价格精度tick为单位，以标记价格为基准。<br>设置makerPxBand为1个tick代表: <br>如果买一价  > 标记价格 + 1 tick, 操作将被拦截 <br>如果 买一价 < 标记价格 - 1 tick, 操作将被拦截 |
