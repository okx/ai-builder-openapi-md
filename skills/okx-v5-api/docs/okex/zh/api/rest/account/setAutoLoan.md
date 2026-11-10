---
site: okex
lang: zh
name: 设置自动借币
method: POST
path: /api/v5/account/set-auto-loan
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 设置自动借币 [trading-account-rest-api-set-auto-loan]

仅适用于跨币种保证金模式和组合保证金模式

#### 限速：5次/2s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/set-auto-loan`

> 请求示例

```shell
POST /api/v5/account/set-auto-loan
body
{
    "autoLoan":true,
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                     |
| :------ | :----- | :------- |:---------------------------------------------------------------------------------------|
| autoLoan  | Boolean | 否     | 是否自动借币 <br>有效值为`true`, `false`<br>默认为 `true`                                                            |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"autoLoan": true
	}]
}
```

#### 返回参数

| 参数名  | 类型   | 描述       |
| :------ | :----- | :--------- |
| autoLoan   | Boolean | 是否自动借币   |
