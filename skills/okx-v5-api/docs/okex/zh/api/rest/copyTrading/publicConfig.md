---
site: okex
lang: zh
name: GET / 获取跟单配置信息
method: GET
path: /api/v5/copytrading/public-config
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取跟单配置信息 [order-book-trading-copy-trading-get-copy-trading-configuration]

公共接口，获取跟单设置时的参数配置信息

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-config`

> 请求示例

```shell
GET /api/v5/copytrading/public-config?instType=SWAP

```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                      |
|:------------| :----- |:-----|:------------------------------------------------------------------------|
| instType    | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "maxCopyAmt": "1000",
            "maxCopyRatio": "100",
            "maxCopyTotalAmt": "30000",
            "maxSlRatio": "0.75",
            "maxTpRatio": "1.5",
            "minCopyAmt": "20",
            "minCopyRatio": "0.01"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**         | **类型** | **描述**                              |
|:----------------|:-------|:------------------------------------|
| maxCopyAmt      | String | 固定金额跟单时，单笔最大跟随金额                    |
| minCopyAmt      | String | 固定金额跟单时，单笔最小跟随金额                    |
| maxCopyTotalAmt | String  | 最大跟单金额（针对单个带单员），最小跟单金额同`minCopyAmt` |
| minCopyRatio    | String  | 比例跟单的单笔最小比率                         |
| maxCopyRatio    | String  | 比例跟单的单笔最大比率                         |
| maxTpRatio      | String  | 单笔最大止盈比率，最小为 0                      |
| maxSlRatio      | String  | 单笔最大止损比率，最小为 0                      |
