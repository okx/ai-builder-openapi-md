---
site: my
lang: zh
name: 闪兑交易
method: POST
path: /api/v5/asset/convert/trade
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 闪兑交易 [funding-account-rest-api-convert-trade]

闪兑交易前需要先 [询价](/docs-v5/zh/#funding-account-rest-api-estimate-quote)。

<aside class="warning">
闪兑只能使用交易账户中的资产
</aside>

#### 限速：10次/s
#### 限速规则：User ID

同一方向(buy/sell) 1次/5s 交易限制

#### HTTP请求

`POST /api/v5/asset/convert/trade`

> 请求示例

```shell
POST /api/v5/asset/convert/trade
body
{
	"baseCcy": "ETH",
	"quoteCcy": "USDT",
	"side": "buy",
	"sz": "30",
	"szCcy": "USDT",
	"quoteId": "quoterETH-USDT16461885104612381"
}
```

#### 请求参数

| 参数名      | 类型   | 是否必须 | 描述                                                    |
|:---------| :----- | :------- |:------------------------------------------------------|
| quoteId  | String | 是       | 报价ID                                                  |
| baseCcy  | String | 是       | 交易货币币种，如 `BTC-USDT`中的`BTC`                            |
| quoteCcy | String | 是       | 计价货币币种，如 `BTC-USDT`中的`USDT`                           |
| side     | String | 是       | 交易方向<br>`buy`：买<br>`sell`：卖<br>描述的是对于`baseCcy`的交易方向       |
| sz       | String | 是       | 用户报价数量<br>报价数量应不大于预估询价中的询价数量                          |
| szCcy    | String | 是       | 用户报价币种                                                |
| clTReqId | String | 否       | 用户自定义的订单标识<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。 |
| tag      | String | 否       | 订单标签<br>适用于broker用户                                   |
| convertMode | String | 否    | `0`：标准闪兑（默认）<br>`1`：VIP大额闪兑 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "baseCcy": "ETH",
            "clTReqId": "",
            "fillBaseSz": "0.01023052",
            "fillPx": "2932.40104429",
            "fillQuoteSz": "30",
            "instId": "ETH-USDT",
            "quoteCcy": "USDT",
            "quoteId": "quoterETH-USDT16461885104612381",
            "side": "buy",
            "state": "fullyFilled",
            "tradeId": "trader16461885203381437",
            "ts": "1646188520338"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名         | 类型   | 描述                                           |
|:------------| :----- |:---------------------------------------------|
| tradeId     | String | 成交ID                                         |
| quoteId     | String | 报价ID                                         |
| clTReqId    | String | 用户自定义的订单标识                                   |
| state       | String | 状态<br>`fullyFilled`：交易成功<br>`rejected`：交易失败  |
| instId      | String | 币对，如 `BTC-USDT`                              |
| baseCcy     | String | 交易货币币种，如 `BTC-USDT`中`BTC`                    |
| quoteCcy    | String | 计价货币币种，如 `BTC-USDT`中`USDT`                   |
| side        | String | 交易方向<br>买：`buy` 卖：`sell`                     |
| fillPx      | String | 成交价格，单位为计价币                                  |
| fillBaseSz  | String | 成交的交易币数量                                     |
| fillQuoteSz | String | 成交的计价币数量                                     |
| ts          | String | 闪兑交易时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
