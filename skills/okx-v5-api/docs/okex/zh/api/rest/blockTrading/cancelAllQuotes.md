---
site: okex
lang: zh
name: 取消所有报价单
method: POST
path: /api/v5/rfq/cancel-all-quotes
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 取消所有报价单 [block-trading-rest-api-cancel-all-quotes]

取消所有报价单

#### 限速: 2次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/cancel-all-quotes`

> 请求示例

```shell
POST /api/v5/rfq/cancel-all-quotes

```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 取消所有报价单
result = blockTradingAPI.cancel_all_quotes()
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
            "ts":"1697026383085"
        }
    ]
}
```

#### 响应参数

| 参数名  | 类型   | 描述                                       |
| :------ | :----- |:-----------------------------------------|
| code	    |String	            | 结果代码，0 表示成功。                             |
| msg	    |String	            | 错误信息，如果代码不为 0，则不为空。                      |
| data	    |Array of objects   | 包含结果的对象数组                                |
| > ts	    |String	            | 成功取消时间，Unix时间戳的毫秒数格式，如 1597026383085。 |
