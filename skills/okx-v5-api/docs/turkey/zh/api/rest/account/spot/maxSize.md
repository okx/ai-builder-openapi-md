---
site: turkey
lang: zh
name: 获取最大可下单数量
method: GET
path: /api/v5/account/max-size
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取最大可下单数量 [trading-account-rest-api-get-maximum-order-quantity]

获取最大可下单数量，可对应下单时的 "sz" 字段

#### 限速：20次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/max-size`

> 请求示例

```shell
GET /api/v5/account/max-size?instId=BTC-USDT&tdMode=cash

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
    tdMode="cash"
)
print(result)
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述**                                                                              |
| :--------- | :------- | :------- |:------------------------------------------------------------------------------------|
| instId     | String   | 是       | 产品ID，如 `BTC-USDT`<br>支持同一业务线下的多产品ID查询（不超过5个），半角逗号分隔                                        |
| tdMode     | String   | 是       | 交易模式<br>`cash`：非保证金                                       |
| px         | String   | 否       | 委托价格<br>当指定多个产品ID查询时，忽略该参数，当未填写处理         |

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

| **参数名** | **类型** | **描述**            |
| :--------- | :------- |:------------------|
| instId     | String   | 产品ID              |
| ccy        | String   | 保证金币种             |
| maxBuy     | String   | `币币`：最大可买的交易币数量   |
| maxSell    | String   | `币币`：最大可卖的计价币数量   |
