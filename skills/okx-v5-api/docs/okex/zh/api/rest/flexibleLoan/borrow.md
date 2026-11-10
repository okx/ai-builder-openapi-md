---
site: okex
lang: zh
name: POST / 下单
method: POST
path: /api/v5/finance/flexible-loan/borrow
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 下单 [financial-product-flexible-loan-post-borrow]

#### 限速：2次/2秒
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/finance/flexible-loan/borrow`

创建一笔活期借币订单，或为已有活跃订单追加借款。

> 请求示例

```shell
POST /api/v5/finance/flexible-loan/borrow
body
{
  "loanData": {"ccy": "USDT", "amt": "1000"},
  "collateralData": [{"ccy": "BTC", "amt": "0.02"}],
  "eMode": "0",
  "clOrdId": "my-borrow-001"
}
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
|:--|:--|:--|:--|
| ordId | String | 否 | 已有订单 ID。省略则创建订单。 |
| loanData | Object | 是 | 借入币种与金额。 |
| > ccy | String | 是 | 借入币种。 |
| > amt | String | 是 | 借入数量。提供 `ordId` 时为追加数量。 |
| collateralData | Array of objects | 按情况 | 抵押资产。创建订单时必填，数量为 1–10 项；追加借款时可选，可与借款一并追加抵押品。 |
| > ccy | String | 是 | 抵押币种。 |
| > amt | String | 是 | 抵押数量。 |
| eMode | String | 按情况 | `0`：不开启（默认）；`1`：开启。仅创建订单时可提供；每个抵押币种/借入币种组合必须受 E-Mode 配对信息接口支持。 |
| clOrdId | String | 是 | 客户端订单 ID，也是 UID 与接口路径维度的幂等键。记录保留 60 秒；已完成的重复请求返回 `51016`，执行中的重复请求返回 `51784`。 |

提供 `ordId` 时，不可提供 `eMode`；订单创建后 E-Mode 状态不可修改。`collateralData` 为可选，提供后会向已有订单追加抵押品。对于 E-Mode 订单，追加的抵押币种必须符合该订单的 E-Mode 配对。

> 返回示例

```json
{
  "code": "0",
  "data": [{"ordId": "680234819012345678", "eMode": "0", "clOrdId": "my-borrow-001"}],
  "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述** |
|:--|:--|:--|
| ordId | String | 新建订单的订单 ID，或被追加借款的订单 ID。 |
| eMode | String | 订单的 E-Mode 状态。 |
| clOrdId | String | 从请求中回显的客户端订单 ID。 |
