---
site: okex
lang: zh
name: 设置 MMP
method: POST
path: /api/v5/account/mmp-config
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 设置 MMP [trading-account-rest-api-set-mmp]

可以使用该接口进行 MMP 的配置。<br>
仅适用于组合保证金账户模式下的期权订单，且有 MMP 权限。

<aside class="notice"><br>
什么是MMP?<br>
做市商保护(MMP)机制保护做市商在一定时间内成交过多。当做市商保护触发时，即做市商在一定时间内(`timeInterval`)成交超过某阈值(`qtyLimit`)，系统会自动撤销所有MMP挂单(`mmp`和`mmp_and_post_only`挂单)，拒绝任何新的MMP订单直到某个时间(MMP最近一次触发时间+`frozenInterval`)或做市商主动重置。<br>
<br>
如何申请MMP?<br>
请发邮件至 institutional@okx.com 或者联系您的客户经理进行申请。
</aside>

<aside class="notice">
MMP 按交易品种（<code>instFamily</code>）单独配置。为某一交易品种启用 MMP <b>不会</b>自动延伸至其他品种。例如，为 <code>BTC-USD</code> 配置 MMP 并不涵盖 <code>ETH-USD</code> 或 <code>SOL-USD</code>，需分别调用此接口为每个品种单独设置。
</aside>

#### 限速：2次/10s
#### 限速规则：User ID
#### HTTP请求

`POST /api/v5/account/mmp-config`

> 请求示例

```shell
POST /api/v5/account/mmp-config
body
{
    "instFamily":"BTC-USD",
    "timeInterval":"5000",
    "frozenInterval":"2000",
    "qtyLimit": "100"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                 |
| :------ | :----- |:-----|:---------------------------------------------------|
| instFamily  | String | 是    | 交易品种                                               |
| timeInterval  | String | 是  | 时间窗口 (毫秒)。<br>"0" 代表停用 MMP                       |
| frozenInterval  | String | 是  | 冻结时间长度 (毫秒)。 <br>"0" 代表一直冻结，直到调用 "重置 MMP 状态" 接口解冻 |
| qtyLimit  | String | 是  | 成交数量的上限<br>需大于 0                                  |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "frozenInterval":"2000",
        "instFamily":"BTC-USD",
        "qtyLimit": "100",
        "timeInterval":"5000"
    }
  ]
}
```

#### 返回参数

| 参数名 | 类型   | 描述          |
|:----| :----- |:------------|
| instFamily     | String | 交易品种        |
| timeInterval     | String | 时间窗口 (毫秒)   |
| frozenInterval     | String | 冻结时间长度 (毫秒) |
| qtyLimit     | String | 成交张数的上限     |
