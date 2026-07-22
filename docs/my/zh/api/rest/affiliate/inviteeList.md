---
site: my
lang: zh
name: 获取直客列表
method: GET
path: /api/v5/affiliate/invitee/list
module: affiliate
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 获取直客列表 [affiliate-rest-api-get-invitee-list]

分页获取直客列表，包含交易统计与 KYC 信息。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/invitee/list`

> 请求示例

```shell
GET /api/v5/affiliate/invitee/list?page=1&kycStatus=verified
```

#### 请求参数

| **参数名**           | **类型** | **是否必须** | **描述** |
| :----------------- | :------- | :---------- | :------- |
| page               | String   | 否          | 1 起始的页码，非数字回退为 `1`。默认为 `1`。 |
| limit              | String   | 否          | 每页数量，限定在 `[1, 100]`。默认为 `100`。 |
| periodType         | String   | 否          | 统计窗口。<br>`last_7d`<br>`last_30d`<br>`this_month`<br>`last_month`<br>`total`<br>`today`<br>`this_week`<br>`custom`：自定义窗口，需配合 `begin` 与 `end` 使用。 |
| begin              | String   | 条件必填     | 自定义统计窗口起始时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `end` 同时传入。包含端点。 |
| end                | String   | 条件必填     | 自定义统计窗口结束时间，Unix时间戳的毫秒数格式。当 `periodType=custom` 时必填，需与 `begin` 同时传入。包含端点。 |
| keyword            | String   | 否          | 按直客 UID 或渠道名搜索。 |
| commissionCategory | String   | 否          | 返佣计算类别。<br>`SPOT`<br>`DERIVATIVE`<br>`BSC` |
| orderBy            | String   | 否          | 排序字段。<br>`cTime`<br>`depAmt`<br>`vol`<br>`fee`<br>`rebate`<br>默认为 `cTime` |
| orderDir           | String   | 否          | 排序方向。<br>`asc`<br>`desc`<br>默认为 `desc` |
| kycStatus          | String   | 否          | KYC 状态。<br>`unverified`：未通过<br>`verified`：至少通过 KYC2 |
| subAffiliateUid    | String   | 否          | 按指定二级节点（外部 UID）筛选其直客。 |

当 `periodType=custom` 时，需同时传 `begin` 和 `end`，仅传一个会返回 `50014`。<br>
其他 `periodType` 值使用服务端预设窗口，与之同时传入的 `begin` / `end` 将被忽略。`begin` 与 `end` 区间不得超过 90 天，`begin` 不得早于当前时间 180 天前。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "5",
    "data": [
        {
            "uid": "835449167911924693",
            "country": "CN",
            "joinTime": "1777448564000",
            "firstTradeTime": "",
            "channelName": "X2UWA2T89",
            "rebateRate": "0.1600",
            "feeTierRank": "0",
            "kycStatus": "verified",
            "kycTime": "1777448563000",
            "depAmt": "0.0000000000",
            "totalVol": "0.0000000000",
            "totalFee": "0.0000000000",
            "totalCommission": "0.0000000000",
            "isCompliant": true
        }
    ]
}
```

#### 返回参数

| **参数名**         | **类型**  | **描述** |
| :--------------- | :------- | :------- |
| totalPage        | String   | 当前过滤条件与 `limit` 下的总页数，与 `data` 在响应中同级。 |
| uid              | String   | 直客的外部 UID。 |
| country          | String   | 直客所在地的 ISO 3166-1 alpha-2 国家/地区码，如 `KR`、`CN`。未设置时返回空字符串。 |
| joinTime         | String   | 返佣关系建立时间，Unix时间戳的毫秒数格式。 |
| firstTradeTime   | String   | 首次交易时间，Unix时间戳的毫秒数格式。如未交易，返回 `""`。 |
| channelName      | String   | 注册时使用的节点渠道名。 |
| rebateRate       | String   | 直客在当前返佣规则下的实际返佣比例（小数形式），如 `0.1000` 表示 `10%`。 |
| feeTierRank      | String   | 跨类别手续费等级排名整数（`0` 最低，`13` 最高）。不区分常规/VIP——分类标签请使用 [获取被邀请人返佣信息](/docs-v5/zh/#affiliate-rest-api-get-the-invitee-39-s-detail) 中的 `level`。 |
| kycStatus        | String   | KYC 状态。<br>`unverified`<br>`verified` |
| kycTime          | String   | KYC2 认证时间，Unix时间戳的毫秒数格式。如未通过 KYC2，返回 `""`。 |
| depAmt           | String   | 累计充值金额，单位为 `USDT`。 |
| totalVol         | String   | 累计交易量，单位为 `USDT`。 |
| totalFee         | String   | 累计交易手续费，单位为 `USDT`。 |
| totalCommission  | String   | 来自该直客的累计返佣，单位为 `USDT`。 |
| isCompliant      | Boolean  | 该直客是否符合区域合规要求。<br>`true`：无限制<br>`false`：因 KYC 主体或司法辖区受限（如制裁地区） |
