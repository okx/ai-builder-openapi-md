---
site: okex
lang: zh
name: 查看 MMP 配置
method: GET
path: /api/v5/account/mmp-config
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 查看 MMP 配置 [trading-account-rest-api-get-mmp-config]

可以使用该接口获取 MMP 的配置信息。<br>
仅适用于组合保证金账户模式下的期权订单，且有 MMP 权限。

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/mmp-config`

> 请求示例

```shell
GET /api/v5/account/mmp-config?instFamily=BTC-USD

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                       |
| :------ | :----- |:-----| :----------------------------------------- |
| instFamily  | String | 否    | 交易品种                      |

> 返回结果

```json
{
  "code": "0",
  "data": [
    {
      "frozenInterval": "2000",
      "instFamily": "ETH-USD",
      "mmpFrozen": true,
      "mmpFrozenUntil": "1000",
      "qtyLimit": "10",
      "timeInterval": "5000"
    }
  ],
  "msg": ""
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                                      |
| :--------- | :------- |:----------------------------------------------------------------------------|
| instFamily     | String   | 交易品种                                                                        |
| mmpFrozen    | Boolean   | 是否 MMP 被触发. `true` 或者 `false`                                               |
| mmpFrozenUntil    | String   | 如果配置了frozenInterval且mmpFrozen = true，则为不再触发MMP时的时间窗口（单位为ms），否则为“”           |
| timeInterval      | String   | 时间窗口 (毫秒)                                                                   |
| frozenInterval      | String   | 冻结时间长度 (毫秒)。 <br>如果为"0"，代表一直冻结，直到调用 "重置 MMP 状态" 接口解冻，且`mmpFrozenUntil`为 ""。 |
| qtyLimit      | String   | 成交张数的上限                                                                     |
