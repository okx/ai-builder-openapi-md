---
site: my
lang: zh
name: GET / 获取市场借贷信息（私有）
method: GET
path: /api/v5/finance/savings/lending-rate-summary-priv
module: finance
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### GET / 获取市场借贷信息（私有）[financial-product-savings-get-public-borrow-info-private]

这是一个私有接口，用户需要通过鉴权才能访问。

获取用户所在站点的市场借贷信息。

#### 限速：6次/s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/finance/savings/lending-rate-summary-priv`

> 请求示例

```shell
GET /api/v5/finance/savings/lending-rate-summary-priv

```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| ccy  | String | 否       | 币种，如 `BTC` |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"ccy": "BTC",
		"avgAmt": "10000",
		"avgAmtUsd": "10000000000",
		"avgRate": "0.03",
		"preRate": "0.02",
		"estRate": "0.01"
	}]
}
```

#### 返回参数

| 参数名    | 类型   | 描述               |
| :-------- | :----- |:-----------------|
| ccy       | String | 币种，如 `BTC`       |
| avgAmt       | String | ~~过去24小时平均借贷量~~(已弃用)      |
| avgAmtUsd  | String | ~~过去24小时平均借贷美元价值~~(已弃用)   |
| avgRate  | String | 过去24小时平均借入年利率 |
| preRate  | String | 上一次借入年利率         |
| estRate  | String | 下一次预估借入年利率       |
