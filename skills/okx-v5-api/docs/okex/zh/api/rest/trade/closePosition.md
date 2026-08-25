---
site: okex
lang: zh
name: POST / 市价仓位全平
method: POST
path: /api/v5/trade/close-position
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 市价仓位全平 [order-book-trading-trade-post-close-positions]

市价平掉指定交易产品的持仓

#### 限速：20次/2s
#### 限速规则（期权以外）：User ID + Instrument ID
#### 限速规则（只限期权）：User ID + Instrument Family

#### HTTP请求

`POST /api/v5/trade/close-position`

> 请求示例

```shell
POST /api/v5/trade/close-position
body
{
    "instId":"BTC-USDT-SWAP",
    "mgnMode":"cross"
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 市价全平
result = tradeAPI.close_positions(
    instId="BTC-USDT-SWAP",
    mgnMode="cross"
)
print(result)
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                            |
| :------ | :----- |:-----|:----------------------------------------------------------------------------------------------|
| instId  | String | 是    | 产品ID                                                                                          |
| posSide | String | 可选   | 持仓方向  <br>买卖模式下：可不填写此参数，默认值net，如果填写，仅可以填写net<br>开平仓模式下： 必须填写此参数，且仅可以填写  `long`：平多 ，`short`：平空 |
| mgnMode | String | 是    | 保证金模式<br>`cross`：全仓 ； `isolated`：逐仓                                                           |
| ccy     | String | 可选   | 保证金币种，`合约模式`下的全仓币币杠杆平仓必填                                                                   |
| autoCxl | Boolean| 否    | 当市价全平时，平仓单是否需要自动撤销,默认为false.<br>`false`：不自动撤单 `true`：自动撤单                                     |
| clOrdId       | String  | 否    | 客户自定义ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                            |
| tag       | String  | 否    | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                               |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "clOrdId": "",
            "instId": "BTC-USDT-SWAP",
            "posSide": "long",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**   |
| :--------- | :------- | :--------- |
| instId     | String   | 产品ID |
| posSide    | String   | 持仓方向   |
| clOrdId       | String  | 客户自定义ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                       |
| tag       | String  | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                               |

<aside class="notice">
如果不自动撤单，那有任何平仓挂单的情况下，市价全平会返回错误码信息，提示用户先撤销平仓挂单<br>
</aside>
