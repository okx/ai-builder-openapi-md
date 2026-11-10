---
site: okex
lang: zh
name: 设置手续费计价方式
method: POST
path: /api/v5/account/set-fee-type
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置手续费计价方式 [trading-account-rest-api-set-fee-type]

设置手续费计价方式。

<aside class="notice">
手續費計價方式選擇對現貨生效。
</aside>

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/account/set-fee-type`

> 请求示例

```shell
POST /api/v5/account/set-fee-type 
body
{
    "feeType": "0"
}
```

#### 请求参数

| 参数名 | 类型 | 是否必须 | 描述 |
|:-------|:-----|:---------|:-----|
| feeType | String | 是 | 手续费计价方式<br>`0`: 按交易获得的币种收取手续费（默认）<br>`1`: 始终按交易对的计价币种收取手续费（仅适用于现货） |

> 返回示例

```json
{
	"code": "0",
	"msg": "",
	"data": [
		{
			"feeType": "0"
		}
	]
}
```

#### 返回参数

| 参数名 | 类型 | 描述 |
|:-------|:-----|:-----|
| feeType | String | 手续费计价方式<br>`0`: 按交易获得的币种收取手续费<br>`1`: 始终按交易对的计价币种收取手续费 |
