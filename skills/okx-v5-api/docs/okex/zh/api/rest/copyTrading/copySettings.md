---
site: okex
lang: zh
name: GET / 获取跟单设置
method: GET
path: /api/v5/copytrading/copy-settings
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取跟单设置 [order-book-trading-copy-trading-get-copy-settings]

获取针对某个交易员的跟单设置

#### 限速：5次/2s  
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/copytrading/copy-settings`

> 请求示例

```shell
GET /api/v5/copytrading/copy-settings?instType=SWAP&uniqueCode=25CD5A80241D6FE6

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                |
| :------ | :----- |:-----|:--------------------------------------------------|
| instType     | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                                               |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位） |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "copyAmt": "",
            "copyInstIdType": "copy",
            "copyMgnMode": "isolated",
            "copyMode": "ratio_copy",
            "copyRatio": "1",
            "copyState": "1",
            "copyTotalAmt": "500",
            "instIds": [
                {
                    "enabled": "1",
                    "instId": "ADA-USDT-SWAP"
                },
                {
                    "enabled": "1",
                    "instId": "YFII-USDT-SWAP"
                }
            ],
            "slRatio": "",
            "slTotalAmt": "",
            "subPosCloseType": "copy_close",
            "tpRatio": "",
            "tag": ""
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**        | **类型** | **描述**                                                                             |
|:---------------| :------- |:-----------------------------------------------------------------------------------|
| copyMode       | String   | 跟单模式<br>`fixed_amount`: 固定金额跟单<br>`ratio_copy`: 比例跟单                               |
| copyAmt        | String   | 单笔跟随金额，单位为 USDT                                                                    |
| copyRatio      | String   | 跟单比例                                                                               |
| copyTotalAmt      | String   | 跟单该交易员投入的最大跟单金额，单位为USDT                                                            |
| tpRatio        | String   | 单笔止盈百分比，0.1 代表10%                                                                  |
| slRatio        | String   | 单笔止损百分比，0.1 代表10%                                                                  |
| copyInstIdType | String   | 跟单合约设置的类型<br>`custom`: 用户自定义 <br>`copy`: 跟随交易员，自动同步交易员的合约变更                        |
| instIds        | Array of objects   | 可跟单的合约列表，会返回交易员所有带单合约                                                              |
| > instId             | String   | 产品 ID                                                                              |
| > enabled         | String   | 是否在跟单<br>`0`: 没有在跟单 `1`: 在跟单                                                       |
| slTotalAmt      | String   | 跟单止损总金额，单位为 USDT                                                                   |
| subPosCloseType     | String   | 剩余仓位处理方式<br>`market_close`: 立即市价全平<br>`copy_close`：跟随交易员平仓<br>`manual_close`: 手动处理 |
| copyMgnMode            | String   | 跟单时的保证金模式<br>`cross`: 全仓；<br>`isolated`: 逐仓；<br>`copy`: 跟随带单员                      |
| ccy       | String   | 保证金币种                                                                              |
| copyState       | String     | 当前跟单状态 <br>`0`: 没在跟单<br>`1`：在跟单                                                    |
| tag         | String | 订单标签                                                                               |
