---
site: okex
lang: zh
name: 获取交割预估结算价格
method: GET
path: /api/v5/public/estimated-settlement-info
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取交割预估结算价格 [public-data-rest-api-get-estimated-future-settlement-price]

获取交割合约预估结算价。只有结算前一小时才有返回值。

#### 限速：10次/2s
#### 限速规则：IP + Instrument ID

#### HTTP请求

`GET /api/v5/public/estimated-settlement-info`

> 请求示例

```shell
GET /api/v5/public/estimated-settlement-info?instId=XRP-USDT-250307
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                      |
| :----- | :----- | :------- |:----------------------------------------|
| instId | String | 是       | 产品ID，如 `XRP-USDT-250307`<br>仅适用于`交割`    |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "estSettlePx": "2.5666068562369959",
            "instId": "XRP-USDT-250307",
            "nextSettleTime": "1741248000000",
            "ts": "1741246429748"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**          | **类型** | **描述**                                   |
|:-----------------| :------- |:-----------------------------------------|
| instId           | String   | 产品ID， 如 `XRP-USDT-250307`                |
| nextSettleTime   | String   | 下一次结算时间，Unix时间戳的毫秒数格式，如 `1597026383085`  |
| estSettlePx      | String   | 预估结算价格                                   |
| ts               | String   | 数据返回时间，Unix时间戳的毫秒数格式，如 `1597026383085`   |
