---
site: hongkong
lang: zh
name: 获取闪兑币对信息
method: GET
path: /api/v5/asset/convert/currency-pair
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取闪兑币对信息 [funding-account-rest-api-get-convert-currency-pair]

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/convert/currency-pair`

> 请求示例

```shell
GET /api/v5/asset/convert/currency-pair?fromCcy=USDT&toCcy=BTC

```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| fromCcy  | String | 是    | 消耗币种，如 `USDT`  |
| toCcy  | String | 是   | 获取币种，如 `BTC`  |
| convertMode | String | 否 | `0`：标准闪兑（默认）<br>`1`：VIP大额闪兑 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "baseCcy": "BTC",
            "baseCcyMax": "0.5",
            "baseCcyMin": "0.0001",
            "instId": "BTC-USDT",
            "quoteCcy": "USDT",
            "quoteCcyMax": "10000",
            "quoteCcyMin": "1"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名      | 类型    | 描述                                                                                        |
| ----------- | ------- | ------------------------------------------------------------------------------------------- |
| instId         | String  | 币对，如 `BTC-USDT`                                                                          |
| baseCcy         | String  | 交易货币币种，如 `BTC-USDT`中的`BTC`                                                            |
| baseCcyMax         | String  | 交易货币支持闪兑的最大值                                                                     |
| baseCcyMin         | String  | 交易货币支持闪兑的最小值                                                                     |
| quoteCcy         | String  | 计价货币币种，如 `BTC-USDT`中的`USDT`                                                            |
| quoteCcyMax         | String  | 计价货币支持闪兑的最大值                                                                     |
| quoteCcyMin         | String  | 计价货币支持闪兑的最小值                                                                     |
