---
site: okex
lang: zh
name: 获取报价方信息
method: GET
path: /api/v5/rfq/counterparties
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取报价方信息 [block-trading-rest-api-get-counterparties]

查询可以参与交易的报价方信息。

#### 限速: 5次/2s
#### 限速规则：User ID
#### HTTP请求

`GET /api/v5/rfq/counterparties`

> 请求示例

```shell
GET /api/v5/rfq/counterparties

```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 获取报价方信息
result = blockTradingAPI.counterparties()
print(result)
```

#### 请求参数
无

> 响应示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "traderName" : "Satoshi Nakamoto",
            "traderCode" : "SATOSHI",
            "type" : "" 
        }
    ]
}
```

#### 响应参数

| 参数名  | 类型   | 描述                                    |
| :------ | :----- |:--------------------------------------|
| traderName| String| 报价方名称                                 |
| traderCode| String| 报价方唯一标识代码，公开可见；报价和询价的相关接口都使用该代码代表报价方。 |
| type		| String| 报价方类型。`LP`指通过API连接的自动做市商。                  |
