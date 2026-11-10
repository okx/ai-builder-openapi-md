---
site: okex
lang: zh
name: GET / 获取带单产品
method: GET
path: /api/v5/copytrading/instruments
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取带单产品 [order-book-trading-copy-trading-get-leading-instruments]

获取平台支持带单的产品，以及获取带单员正在带单的产品

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/instruments`

> 请求示例

```shell
GET /api/v5/copytrading/instruments

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- |:-----| :----------------------------------------------------------- |
| instType     | String | 否       | 产品类型<br>SPOT：币币<br>SWAP：永续合约，默认值                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "enabled": true,
            "instId": "BTC-USDT-SWAP"
        },
        {
            "enabled": true,
            "instId": "ETH-USDT-SWAP"
        },
        {
            "enabled": false,
            "instId": "ADA-USDT-SWAP"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**  | **类型** | **描述**                   |
| :---------- | :------- |:-------------------------|
| instId      | String   | 产品ID                     |
| enabled      | Boolean   | 是否设置了带单 `true` 或 `false` |
