---
site: okex
lang: zh
name: 获取最大可下单数量
method: GET
path: /api/v5/account/max-size
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取最大可下单数量 [trading-account-rest-api-get-maximum-order-quantity]

获取最大可下单数量，可对应下单时的 "sz" 字段

<aside class="notice">
Portfolio Margin 账户下，衍生品的全仓模式不支持最大可买卖/开仓数量的计算。
</aside>

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/max-size`

> 请求示例

```shell
GET /api/v5/account/max-size?instId=BTC-USDT&tdMode=isolated

```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取最大可买卖/开仓数量
result = accountAPI.get_max_order_size(
    instId="BTC-USDT",
    tdMode="isolated"
)
print(result)
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述**                                                                          |
| :--------- | :------- | :------- |:--------------------------------------------------------------------------------|
| instId     | String   | 是       | 产品ID，如 `BTC-USDT`<br>支持同一业务线下的多产品ID查询（不超过5个），半角逗号分隔                             |
| tdMode     | String   | 是       | 交易模式<br>`cross`：全仓<br>`isolated`：逐仓<br>`cash`：非保证金<br>`spot_isolated`：现货逐仓，仅适用于`合约模式` |
| ccy        | String   | 可选     | 保证金币种，适用于`逐仓杠杆`及`合约模式`下的`全仓杠杆`订单                                             |
| px         | String   | 否       | 委托价格<br>当不填委托价时，交割和永续会取当前限价计算，其他业务线会按当前最新成交价计算<br>当指定多个产品ID查询时，忽略该参数，当未填写处理     |
| leverage   | String   | 否       | 开仓杠杆倍数<br>默认为当前杠杆倍数<br>仅适用于`币币杠杆/交割/永续`                                         |
| tradeQuoteCcy    | String | 否       | 用于交易的计价币种。仅适用于`币币`。<br>默认值为 `instId` 的计价币种，比如：对于 `BTC-USD`，默认取 `USD`。 |
| outcome | String | 可选 | 交易的市场结果方向。<br>`yes`<br>`no`<br>仅适用于 `EVENTS`，选填，默认值为`yes` |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"instId": "BTC-USDT",
		"maxBuy": "0.0500695098559788",
		"maxSell": "64.4798671570072269"
    }]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instId     | String   | 产品ID                                                   |
| ccy        | String   | 保证金币种                     |
| maxBuy     | String   | `币币/币币杠杆`：最大可买的交易币数量<br>`合约模式`下的全仓杠杆订单，为交易币数量<br>`交割`/`永续`/`期权`：最大可开多的合约张数 |
| maxSell    | String   | `币币/币币杠杆`：最大可卖的计价币数量<br>`合约模式`下的全仓杠杆订单，为交易币数量<br>`交割`/`永续`/`期权`：最大可开空的合约张数 |
