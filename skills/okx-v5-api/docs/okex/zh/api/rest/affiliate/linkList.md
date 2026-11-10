---
site: okex
lang: zh
name: 获取邀请链接列表
method: GET
path: /api/v5/affiliate/link/list
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 获取邀请链接列表 [affiliate-rest-api-get-link-list]

分页获取节点的邀请链接，包括返佣比例与统计数据。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/link/list`

> 请求示例

```shell
GET /api/v5/affiliate/link/list
```

#### 请求参数

| **参数名**   | **类型** | **是否必须** | **描述** |
| :---------- | :------- | :---------- | :------- |
| page        | String   | 否          | 1 起始的页码，非数字回退为 `1`。默认为 `1`。 |
| limit       | String   | 否          | 每页数量，限定在 `[1, 100]`。默认为 `100`。 |
| linkType    | String   | 否          | 链接类型筛选。<br>`standard`：常规节点邀请链接<br>`co_inviter`：联合邀请人共享链接 |
| linkStatus  | String   | 否          | 链接状态。<br>`normal`<br>`abnormal` |

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "channelId": "78295211",
            "channelName": "78295211",
            "joinLink": "https://okx.com/join/78295211",
            "linkType": "standard",
            "inviterCommissionRate": "0.2900",
            "coInviterCommissionRate": "",
            "inviteeDiscountRate": "0.0100",
            "inviteeCnt": "1",
            "traderCnt": "1",
            "totalCommission": "2.656307",
            "commission24h": "0.5",
            "cTime": "1773739123000",
            "isDefault": true,
            "linkStatus": "normal"
        }
    ]
}
```

#### 返回参数

| **参数名**                | **类型**  | **描述** |
| :----------------------- | :------- | :------- |
| totalPage                | String   | 当前过滤条件与 `limit` 下的总页数，与 `data` 在响应中同级。 |
| channelId                | String   | 渠道/链接唯一 ID。 |
| channelName              | String   | 用户自定义的链接名称。 |
| joinLink                 | String   | 可分享的邀请 URL。 |
| linkType                 | String   | 链接类型。<br>`standard`：常规节点邀请链接<br>`co_inviter`：联合邀请人共享链接 |
| inviterCommissionRate    | String   | 父级邀请人（链接所有者）的返佣比例，小数形式。 |
| coInviterCommissionRate  | String   | 联合邀请人的返佣比例，小数形式。常规链接为空字符串。 |
| inviteeDiscountRate      | String   | 该链接配置的直客返佣比例模板（小数形式），适用于通过该链接注册的直客。 |
| inviteeCnt               | String   | 通过该链接邀请的直客数。 |
| traderCnt                | String   | 已交易的直客数。 |
| totalCommission          | String   | 累计返佣，单位为 `USDT`。 |
| commission24h            | String   | 滚动近 24 小时返佣，单位为 `USDT`。不受 `periodType` / `begin` / `end` 过滤影响。 |
| cTime                    | String   | 链接创建时间，Unix时间戳的毫秒数格式。 |
| isDefault                | Boolean  | 是否为默认链接。 |
| linkStatus               | String   | 链接状态。<br>`normal`<br>`abnormal` |
