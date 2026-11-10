---
site: my
lang: zh
name: 获取 TVB 被邀请人详情
method: GET
path: /api/v5/affiliate/tvb/invitee/detail
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com
supported: true
---

### 获取 TVB 被邀请人详情 [affiliate-rest-api-get-tvb-invitee-detail]

本节数据适用于采用 TVB 激励模式的节点账户。在 TVB（交易量返佣，Trading Volume Bonus）模式下，奖励根据来自适用司法辖区的被邀请人所产生的符合条件的交易量计算。本接口返回特定被邀请人的 TVB 业绩详情。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/tvb/invitee/detail`

> 请求示例

```shell
GET /api/v5/affiliate/tvb/invitee/detail?uid=855082962927686909
```

#### 请求参数

| **参数名** | **类型** | **是否必须** | **描述** |
| :-------- | :------- | :---------- | :------- |
| uid       | String   | 是          | 被邀请人的 UID。 |

通过 `uid` 查询单个被邀请人；无时间窗口参数。<br>
不暴露单个被邀请人的返佣——仅返回交易量、符合条件的交易量与入金额。返佣从不归因于单个被邀请人（仅在节点总额与分档层级呈现）。<br>
对无入金查看权限的调用方，`validDepAmt` 会被掩码为 `""`。<br>
无效或无法解析的 `uid` 返回 HTTP 200，业务码非零且 `data` 为空——不会透露该 UID 是否存在。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "uid": "855082962927686909",
            "inviterUid": "853364004059009741",
            "joinTime": "1782129625000",
            "country": "NL",
            "kycStatus": "verified",
            "kycTime": "1782129625324",
            "affiliateCode": "54022003",
            "note": "VIP client from IG",
            "firstTradeTime": "",
            "level": "Regular User",
            "validVol": "12500.00",
            "eligibleVol": "9800.00",
            "validDepAmt": "1420.11"
        }
    ]
}
```

#### 返回参数

| **参数名**      | **类型** | **描述** | **数据范围** |
| :------------- | :------- | :------- | :------------- |
| uid            | String   | 被邀请人的 UID。 |
| inviterUid     | String   | 被邀请人直接邀请人（邀请该被邀请人的账户）的公开 UID。直接被邀请人时等于节点本身；在 MLRS 下为子节点的 UID。无法解析邀请人时返回 `""`。 |
| joinTime       | String   | 与该节点建立邀请关系的时间，Unix时间戳的毫秒数格式。 |
| country        | String   | 被邀请人的国家 / 地区。 |
| kycStatus      | String   | KYC 状态。<br>`unknown`<br>`unverified`<br>`verified`<br>`video_verified`<br>合规信息不可用时返回 `""`。 |
| kycTime        | String   | KYC 认证时间，Unix时间戳的毫秒数格式。未认证时返回 `""`。 |
| affiliateCode  | String   | 被邀请人注册时使用的节点邀请码。 |
| note           | String   | 节点对该被邀请人的私有备注。无备注时返回 `""`。 |
| firstTradeTime | String   | 建立关系后被邀请人的首次交易时间，Unix时间戳的毫秒数格式。未交易时返回 `""`。 |
| level          | String   | 被邀请人的交易手续费等级展示名称，例如 `Regular User`、`VIP 1`–`VIP 9`。 |
| validVol       | String   | 被邀请人的总交易量，单位为 `USDC`。 | TVB only |
| eligibleVol    | String   | 符合条件的交易量——产生返佣的子集，单位为 `USDC`。 | TVB only |
| validDepAmt    | String   | 被邀请人的总入金额，单位为 `USDC`。对无入金查看权限的调用方返回 `""`（掩码）。 | CPS+TVB both |
