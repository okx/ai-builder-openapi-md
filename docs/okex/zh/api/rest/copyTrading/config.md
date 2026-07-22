---
site: okex
lang: zh
name: GET / 查看账户配置信息
method: GET
path: /api/v5/copytrading/config
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 查看账户配置信息 [order-book-trading-copy-trading-get-account-configuration]

获取跟单交易和带单交易相关的账户配置信息

#### 限速：5次/2s  
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/config`

> 请求示例

```shell
GET /api/v5/copytrading/config

```

#### 请求参数

无

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "details": [
                {
                    "copyTraderNum": "1",
                    "instType": "SWAP",
                    "maxCopyTraderNum": "100",
                    "profitSharingRatio": "0",
                    "roleType": "1"
                },
                {
                    "copyTraderNum": "",
                    "instType": "SPOT",
                    "maxCopyTraderNum": "",
                    "profitSharingRatio": "",
                    "roleType": "0"
                }
            ],
            "nickName": "155***9957",
            "portLink": "",
            "uniqueCode": "5506D3681454A304"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**              | **类型** | **描述**                                  |
|:---------------------|:-------|:----------------------------------------|
| uniqueCode           | String | 交易员唯一标识代码                               |
| nickName             | String | 昵称                                      |
| portLink             | String | 头像的链接地址                                 |
| details              | Array of objects  | 详情                                      |
| > instType           | String | 产品类型<br>`SPOT`: 币币<br>`SWAP`: 永续合约  |
| > roleType           | String | 用户角色<br>`0`：普通用户<br>`1`：带单者<br>`2`：跟单者 |
| > profitSharingRatio | String | 分润比例，仅适用于带单员，0.1 代表 10%，否则为""           |
| > maxCopyTraderNum   | String | 最大跟单人数，仅适用于带单员 |
| > copyTraderNum      | String | 当前跟单人数，仅适用于带单员 |
