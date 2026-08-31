---
site: okex
lang: zh
name: POST / 停止跟单
method: POST
path: /api/v5/copytrading/stop-copy-trading
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 停止跟单 [order-book-trading-copy-trading-post-stop-copying]

该接口用来停止跟单<br>

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/stop-copy-trading`

> 请求示例

```shell
POST /api/v5/copytrading/stop-copy-trading
body
{
    "instType": "SWAP",
    "uniqueCode": "25CD5A80241D6FE6",
    "subPosCloseType": "manual_close"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                     |
| :------ | :----- |:-----|:-------------------------------------------------------------------------------------------------------|
| instType     | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                                               |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位）                                                      |
| subPosCloseType     | String | 可选   | 剩余仓位处理方式，有相关的跟单条目时必填<br>`market_close`: 立即市价全平<br>`copy_close`：跟随交易员平仓<br>`manual_close`: 手动处理<br>|

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

| **参数名**  | **类型** | **描述**                                                                                                                                                                                       |
| :---------- | :------- |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| result      | Boolean   | 设置结果<br>`true`：设置成功                                                                                                                                                                                       |
