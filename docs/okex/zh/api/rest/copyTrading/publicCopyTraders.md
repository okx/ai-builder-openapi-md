---
site: okex
lang: zh
name: GET / 获取跟单人信息
method: GET
path: /api/v5/copytrading/public-copy-traders
module: copytrading
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取跟单人信息 [order-book-trading-copy-trading-get-copy-traders]

公共接口，获取交易员的跟单人信息，按收益从高到低返回

#### 限速：5次/2s  
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/copytrading/public-copy-traders`

> 请求示例

```shell
GET /api/v5/copytrading/public-copy-traders?instType=SWAP&uniqueCode=D9ADEAB33AE9EABD

```

#### 请求参数

| 参数名         | 类型   | 是否必须 | 描述                                                                      |
|:------------| :----- |:-----|:------------------------------------------------------------------------|
| instType    | String | 否    | 产品类型<br>`SWAP`：永续合约，默认值                                                |
| uniqueCode  | String | 是    | 带单交易员唯一标识码。<br>数字加字母组合 长度为16或18位，如：213E8C92DC61EFAC（16位）或381749205163847291（18位）                       |
| limit    | String | 否       | 返回结果的数量，最大为100，默认100条                                   |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "ccy": "USDT",
            "copyTotalPnl": "2060.12242",
            "copyTraderNumChg": "1",
            "copyTraderNumChgRatio": "0.5",
            "copyTraders": [
                {
                    "beginCopyTime": "1686125051000",
                    "nickName": "bre***@gmail.com",
                    "pnl": "1076.77388",
                    "portLink": ""
                },
                {
                    "beginCopyTime": "1698133811000",
                    "nickName": "MrYanDao505",
                    "pnl": "983.34854",
                    "portLink": "https://static.okx.com/cdn/okex/users/headimages/20231010/fd31f45e99fe41f7bb219c0b53ae0ada"
                }
            ]
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**               | **类型** | **描述**         |
|:----------------------|:-------|:---------------|
| copyTotalPnl          | String | 跟单员总收益         |
| ccy                   | String | 总收益币种名称        |
| copyTraderNumChg      | String | 近 7 日变化的跟单人数   |
| copyTraderNumChgRatio | String | 近 7 日跟单人数变化的比率 |
| copyTraders           | Array of objects  | 跟单员信息          |
| > beginCopyTime       | String | 跟单开始时间，Unix时间戳的毫秒数格式，如 `1597026383085`                 |
| > nickName            | String | 昵称             |
| > portLink            | String | 跟单员头像的链接地址     |
| > pnl                 | String | 跟单收益           |
