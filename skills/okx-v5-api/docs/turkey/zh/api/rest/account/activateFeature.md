---
site: turkey
lang: zh
name: 开通功能
method: POST
path: /api/v5/account/activate-feature
module: account
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### 开通功能 [trading-account-rest-api-activate-feature]

若下单返回错误码 `54109`，请调用以下接口为账户开通 USDC 交易功能；否则无需调用该接口。

#### 限速：5 次/2 秒
#### 限速规则：User ID

#### HTTP 请求

`POST /api/v5/account/activate-feature`

> 请求示例

```shell
POST /api/v5/account/activate-feature
body
{
    "feature": "1"
}
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| --- | --- | --- | --- |
| feature | String | 是 | 要开通的具体功能<br>`1`：USDC 订单簿交易功能。<br>仅当下单返回错误码 `54109` 时，才需要调用该功能；否则无需调用。<br>错误码 `51773` 仅表示不支持使用该激活功能。能否交易 `Crypto-USDC` 产品，以是否能够成功下单为准。<br>母账户与子账户之间共享开通状态，母账户或任一子账户仅需调用一次。 |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": []
}
```

#### 返回参数

无
