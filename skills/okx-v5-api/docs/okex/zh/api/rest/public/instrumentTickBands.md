---
site: okex
lang: zh
name: 获取期权价格梯度
method: GET
path: /api/v5/public/instrument-tick-bands
module: public
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取期权价格梯度[public-data-rest-api-get-option-tickbands]

获取产品价格梯度信息

#### 限速：5次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/instrument-tick-bands`

> 请求示例

```shell
GET /api/v5/public/instrument-tick-bands?instType=OPTION
```

```shell
GET /api/v5/public/instrument-tick-bands?instType=EVENTS
```
#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                                        |
|:---------------| :----- |:-----|:--------------------------------------------------------------------------------------------------------------------------|
| instType       | String | 是 | 产品类型<br>`OPTION`：期权<br>`EVENTS`：事件合约                                                                                                             |
| instFamily         | String | 否   | 交易品种，仅适用于 `OPTION`                                                                           |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "OPTION",
            "instFamily": "BTC-USD",
            "tickBand": [
                {
                    "minPx": "0",
                    "maxPx": "100",
                    "tickSz": "0.1"
                },
                {
                    "minPx": "100",
                    "maxPx": "10000",
                    "tickSz": "1"
                }
            ]
        },
        {
            "instType": "OPTION",
            "instFamily": "ETH-USD",
            "tickBand": [
                {
                    "minPx": "0",
                    "maxPx": "100",
                    "tickSz": "0.1"
                },
                {
                    "minPx": "100",
                    "maxPx": "10000",
                    "tickSz": "1"
                }
            ]
        }
    ]
}
```

> 返回结果：EVENTS

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "instType": "EVENTS",
            "instFamily": "",
            "tickBand": [
                {
                    "minPx": "0.001",
                    "maxPx": "0.04",
                    "tickSz": "0.001"
                },
                {
                    "minPx": "0.04",
                    "maxPx": "0.96",
                    "tickSz": "0.01"
                },
                {
                    "minPx": "0.96",
                    "maxPx": "0.999",
                    "tickSz": "0.001"
                }
            ]
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                               |
| :--------- |:-------|:-------------------------------------|
| instType      | String | 产品类型                                 |
| instFamily    | String | 交易品种。仅适用于 `OPTION`                   |
| tickBand      | Array of objects  | 价格梯度。对于 `EVENTS`，返回适用于所有事件合约的统一价格梯度配置。 |
| > minPx       | String | 下单最低价格 |
| > maxPx       | String | 下单最高价格 |
| > tickSz      | String | 下单价格精度，如 `0.0001`               |
