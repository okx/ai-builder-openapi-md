---
site: my
lang: zh
name: 获取被邀请人返佣信息
method: GET
path: /api/v5/affiliate/invitee/detail
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取被邀请人返佣信息 [affiliate-rest-api-get-the-invitee-39-s-detail]

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/invitee/detail`

> 请求示例

```shell
GET /api/v5/affiliate/invitee/detail?uid=11111111&periodType=last_30d
```

#### 请求参数

| 参数名   | 类型   | 是否必须 | 描述                                                 |
|:------| :----- |:-----|:---------------------------------------------------|
| uid   | String | 是    | 被邀请人UID，仅支持使用被邀请人母账号的 UID<br/>返回数据中涵盖了被邀请人母账户和子账户。 |
| periodType | String | 否 | `volPeriod` 的统计窗口。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>不传时不返回 `volPeriod`。本接口不支持 `custom`；`custom` 或任意未知值返回 `51000`。 |

> 返回结果

```json
{
    "msg": "",
    "code": "0",
    "data": [
        {
            "accFee": "0",
            "affiliateCode": "HIIIIII",
            "depAmt": "0",
            "wdAmt": "0",
            "firstTradeTime": "",
            "inviteeLevel": "2",
            "inviteeRebateRate": "0.39",
            "joinTime": "1712546713000",
            "kycTime": "",
            "level": "Lv1",
            "region": "越南",
            "totalCommission": "0",
            "volMonth": "0",
            "totalVol": "0",
            "volPeriod": "1234.56"
        }
    ]
}
```

#### 返回参数

| **参数名**           | **类型** | **描述**                                    |
|:------------------| :------- |:------------------------------------------|
| inviteeLevel      | String   | 被邀请人的节点层级<br>直客返回`2`                      |
| joinTime          | String     | 返佣关系建立的时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| inviteeRebateRate | String     | 返佣比例(小数形式)，如 `0.01`代表`1%`                 |
| totalCommission   | String     | 总返佣数量，单位为`USDT`                             |
| firstTradeTime    | String  | 首次交易时间（在最近的返佣关系建立之后）<br/> Unix时间戳的毫秒数格式，如 1597026383085<br/>如果用户没有交易, 返回 "" |
| level             | String   | 当前在平台上真实交易量的用户等级，如 Lv1                                                     |
| depAmt            | String   | 累计充值金额，单位为 USDT<br/>如果没有充值, 返回 0                                            |
| wdAmt             | String   | 累计提现金额，单位为 USDT<br/>如果没有提现, 返回 0                                            |
| volMonth          | String   | 当月累计交易量，单位为 USDT<br/>如果没有交易, 返回 0                                           |
| totalVol          | String   | 生命周期累计交易量，单位为 USDT<br/>如果没有交易, 返回 0                                        |
| volPeriod         | String   | 所选 `periodType` 窗口内的交易量，单位为 `USDT`。仅当请求传入 `periodType` 时返回，否则省略。窗口内无交易时返回 `0`。 |
| accFee            | String   | 累计交易手续费，单位为 USDT<br/>如果没有交易手续费，返回 0                                         |
| kycTime           | String   | KYC2 认证时间. Unix时间戳的毫秒数格式，且精确到天<br/>如果没有通过 KYC2, 返回 ""                       |
| region            | String   | 国家或地区，如"英国"                                                                 |
| affiliateCode     | String   | 节点邀请码                                                                       |
