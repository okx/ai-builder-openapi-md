---
site: okex
lang: zh
name: POST / 首次跟单设置
method: POST
path: /api/v5/copytrading/first-copy-settings
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### POST / 首次跟单设置 [order-book-trading-copy-trading-post-first-copy-settings]

跟随某一交易员的首次设置，停止跟单后需先进行首次设置；<br>

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/copytrading/first-copy-settings`

> 请求示例

```shell
POST /api/v5/copytrading/first-copy-settings
body
{
    "instType": "SWAP",
    "uniqueCode": "25CD5A80241D6FE6",
    "copyMgnMode": "cross",
    "copyInstIdType": "copy",
    "copyMode": "ratio_copy",
    "copyRatio": "1",
    "copyTotalAmt": "500",
    "subPosCloseType": "copy_close"
}
```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                                                                         |
| :------ | :----- |:-----|:-----------------------------------------------------------------------------------------------------------|
| instType     | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                                                   |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位）                                                          |
| copyMgnMode     | String | 是    | 跟单时的保证金模式<br>`cross`: 全仓；<br>`isolated`: 逐仓；<br>`copy`: 跟随带单员                                           |
| copyInstIdType     | String | 是    | 跟单合约设置的类型<br>`custom`: 用户自定义，instId 必填；<br>`copy`: 跟随交易员，自动同步交易员的合约变更                                    |
| instId     | String | 可选   | 产品 ID<br>可传入多条，以逗号区分                                                                             |
| copyMode     | String | 否    | 跟单模式<br>`fixed_amount`: 固定金额跟单，`copyAmt`必填；<br>`ratio_copy`: 比例跟单，`copyRatio`必填 <br>默认是`fixed_amount`       |
| copyTotalAmt     | String | 是    | 跟单该交易员投入的最大跟单金额，单位为USDT。<br>超过该金额后将不再触发跟单行为                                                                    |
| copyAmt     | String | 可选   | 单笔跟随金额，单位为USDT                                                                                             |
| copyRatio     | String | 可选   | 跟单比例                                                                                                       |
| tpRatio     | String | 否    | 单笔止盈百分比，0.1 代表10%                                                                                          |
| slRatio     | String | 否    | 单笔止损百分比，0.1 代表10%                                                                                          |
| slTotalAmt     | String | 否    | 跟单止损总金额，单位为USDT<br>净损失达到该金额时，将自动解除跟单关系                                                                    |
| subPosCloseType     | String | 是    | 剩余仓位处理方式<br>`market_close`: 立即市价全平<br>`copy_close`：跟随交易员平仓<br>`manual_close`: 手动处理<br>默认为 `copy_close` |
| tag     | String | 否       | 订单标签<br/>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间。                                |

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
