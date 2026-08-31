---
site: okex
lang: zh
name: 获取二级节点列表
method: GET
path: /api/v5/affiliate/sub-affiliate/list
module: affiliate
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取二级节点列表 [affiliate-rest-api-get-sub-affiliate-list]

分页获取当前用户下的二级节点。

#### 限速：3次/s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/affiliate/sub-affiliate/list`

> 请求示例

```shell
GET /api/v5/affiliate/sub-affiliate/list
```

#### 请求参数

| **参数名**           | **类型** | **是否必须** | **描述** |
| :----------------- | :------- | :---------- | :------- |
| page               | String   | 否          | 1 起始的页码，非数字回退为 `1`。默认为 `1`。 |
| limit              | String   | 否          | 每页数量，限定在 `[1, 100]`。默认为 `100`。 |
| keyword            | String   | 否          | 按二级节点 UID 搜索。 |
| commissionCategory | String   | 否          | 返佣计算类别。<br>`SPOT`<br>`DERIVATIVE`<br>`BSC` |
| orderBy            | String   | 否          | 排序字段。<br>`cTime`<br>`depAmt`<br>`vol`<br>`fee`<br>`rebate`<br>默认按 `joinTime` 倒序（最近加入的优先）。 |
| orderDir           | String   | 否          | 排序方向。<br>`asc`<br>`desc`<br>默认为 `desc` |

该接口返回的是生命周期累计数据。<br>
排序具有稳定性：当 `orderBy` 出现并列时，按 `subAffiliateUid` 升序作为次序。可安全分页大数据集，不会丢行或重复。

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "totalPage": "1",
    "data": [
        {
            "subAffiliateUid": "668418489887292061",
            "country": "CN",
            "joinTime": "1773739123000",
            "subAffiliateLevel": "2",
            "commissionRate": "0.3000",
            "isCompliant": true,
            "inviteeCnt": "8",
            "traderCnt": "3",
            "depAmt": "37.281133",
            "totalVol": "3618.561430",
            "totalFee": "1.628353",
            "totalCommission": "0.289847"
        }
    ]
}
```

#### 返回参数

| **参数名**         | **类型**  | **描述** |
| :--------------- | :------- | :------- |
| totalPage        | String   | 当前过滤条件与 `limit` 下的总页数，与 `data` 在响应中同级。 |
| subAffiliateUid  | String   | 二级节点的外部 UID。 |
| country          | String   | 二级节点所在地的 ISO 3166-1 alpha-2 国家/地区码，如 `CN`。未设置时返回空字符串。 |
| joinTime         | String   | 注册为二级节点的时间，Unix时间戳的毫秒数格式。 |
| subAffiliateLevel| String   | 相对调用方的层级深度。<br>`2`：直接二级节点（调用方的 L1 子节点）<br>`3`：间接二级节点，位于直接二级节点下一层 |
| commissionRate   | String   | 二级节点的返佣比例，小数形式。 |
| isCompliant      | Boolean  | 二级节点是否符合区域合规要求。<br>`true`：无限制<br>`false`：因 KYC 主体或司法辖区受限（如制裁地区） |
| inviteeCnt       | String   | 二级节点的直客数。 |
| traderCnt        | String   | 二级节点中已交易的直客数。 |
| depAmt           | String   | 直客累计充值金额，单位为 `USDT`。 |
| totalVol         | String   | 直客累计交易量，单位为 `USDT`。 |
| totalFee         | String   | 直客累计交易手续费，单位为 `USDT`。 |
| totalCommission  | String   | 您从该二级节点直客中获得的累计返佣，单位为 `USDT`。 |
