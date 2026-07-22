---
site: okex
lang: zh
name: POST / 撤销 MMP 订单
method: POST
path: /api/v5/trade/mass-cancel
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 撤销 MMP 订单[order-book-trading-trade-post-mass-cancel-order]

撤销同一交易品种下用户所有的 MMP 挂单<br>
仅适用于组合保证金账户模式下的期权订单，且有 MMP 权限。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/trade/mass-cancel`

> 请求示例

```shell
POST /api/v5/trade/mass-cancel
body
{
    "instType":"OPTION",
    "instFamily":"BTC-USD"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                           |
| :------ | :----- |:-----|:-------------------------------------------------------------------------------------------------------------|
| instType  | String | 是    | 交易产品类型<br>`OPTION`:期权                                                                                       |
| instFamily  | String | 是    | 交易品种                                                                                                         |
| lockInterval | String | 否 | 锁定时长(毫秒)<br> 范围应为[0, 10 000]<br> 默认为 0. 如果想要立即解锁，您可以设置为 "0"<br> 下单时，如果在该锁定期间，会报错 54008，如果在 MMP 触发期间，会报错 51034 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "result":true
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                        |
| :------ | :----- |:------------------------------------------|
| result   | Boolean | 撤单结果<br>`true`：全部撤单成功<br>`false`：全部撤单失败 |
