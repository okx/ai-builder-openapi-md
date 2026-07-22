---
site: okex
lang: zh
name: POST / 修改分润比例
method: POST
path: /api/v5/copytrading/amend-profit-sharing-ratio
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 修改分润比例 [order-book-trading-copy-trading-post-amend-profit-sharing-ratio]

修改分润比例

#### 限速：5次/2s  
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/amend-profit-sharing-ratio`

> 请求示例

```shell
POST /api/v5/copytrading/amend-profit-sharing-ratio
body
{
    "instType": "SWAP",
    "profitSharingRatio": "0.1"
}

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                         |
| :------ | :----- |:-----|:-----------------------------------------------------------|
| instType     | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                   |
| profitSharingRatio     | String | 是    | 分润比例。0.1 代表10%                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "result": true
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**        | **类型** | **描述**                                                                                |
|:---------------| :------- |:--------------------------------------------------------------------------------------|
| result      | Boolean   | 设置结果<br>`true`：设置成功                                                                                                                                                                                       |
