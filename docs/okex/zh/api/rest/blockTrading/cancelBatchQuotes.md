---
site: okex
lang: zh
name: 批量取消报价单
method: POST
path: /api/v5/rfq/cancel-batch-quotes
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 批量取消报价单 [block-trading-rest-api-cancel-multiple-quotes]

取消一个或多个报价单，每次最多可以撤销100个订单。

#### 限速: 2次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/cancel-batch-quotes`

> 请求示例

```shell
POST /api/v5/rfq/cancel-batch-quotes
{
    "quoteIds": ["1150","1151","1152"],
    "clQuoteIds": ["q1","q2","q3"]
}
```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 批量取消报价单
result = blockTradingAPI.cancel_batch_quotes(
    quoteIds=["1150","1151","1152"],
    clQuoteIds=["q1","q2","q3"]
)
print(result)
```

#### 请求参数
| 参数名  | 类型               | 是否必须    | 描述                                                                                |
| :------ |:-----------------| :----- |:----------------------------------------------------------------------------------|
| quoteIds   | Array of strings |可选 | 报价单ID                                                                             |
| clQuoteIds | Array of strings |可选 | 报价单自定义ID，当 clQuoteIds 和 quoteIds 都传时，以 quoteIds 为准。 |

> 全部成功的示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1152",
            "clQuoteId":"q3",
            "sCode":"0",
            "sMsg":""
        }
    ]
}

```

> 部分成功的示例 

```json
{
    "code":"2",
    "msg":"Bulk operation partially succeeded.",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"0",
            "sMsg":""
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1152",
            "clQuoteId":"q3",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        }
    ]
}

```

> 失败示例

```json
{
    "code":"1",
    "msg":"Operation failed.",
    "data":[
        {
            "quoteId":"1150",
            "clQuoteId":"q1",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q2",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        },
        {
            "quoteId":"1151",
            "clQuoteId":"q3",
            "sCode":"70001",
            "sMsg":"Quote does not exist."
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                              |
| :------ | :----- | :-----------------------------------------------------------  |
| code	        |String	            | 结果代码，0 表示成功。|
| msg	        |String	            | 错误信息，如果代码不为 0，则不为空。|
| data	        |Array of objects   | 包含结果的对象数组           |
| > quoteId	    |String	            | 报价单ID                                             |
| > clQuoteId	|String	            | 报价单自定义ID                            |
| > sCode	|String	            | 事件执行结果的code，0代表成功|
| > sMsg	|String	            | 事件执行失败时的msg          |
