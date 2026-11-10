---
site: okex
lang: zh
name: GET / E-Mode 配对信息
method: GET
path: /api/v5/finance/flexible-loan/emode-info
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / E-Mode 配对信息

#### 限速：5次/2秒
#### 限速规则：IP
#### Permission: Read

#### HTTP 请求

`GET /api/v5/finance/flexible-loan/emode-info`

> 请求示例

```shell
GET /api/v5/finance/flexible-loan/emode-info
```

#### 请求参数

无。

返回当前所有支持 E-Mode 的抵押币种与借入币种配对。该接口无需鉴权。

> 返回示例

```json
{
  "code": "0",
  "data": [
    {"collateralCcy": "USDC", "borrowCcy": "USDT"},
    {"collateralCcy": "WBTC", "borrowCcy": "BTC"}
  ],
  "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
|:--|:--|:--|
| collateralCcy | String | 支持 E-Mode 的抵押币种。 |
| borrowCcy | String | 与 `collateralCcy` 配对后支持 E-Mode 的借入币种。 |
