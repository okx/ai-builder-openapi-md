---
site: okex
lang: zh
name: 取消报价单
method: POST
path: /api/v5/rfq/cancel-quote
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 取消报价单 [block-trading-rest-api-cancel-quote]

取消一个报价单。

如果在本次取消请求到达之前，系统已处理了针对同一 `rfqId` 的新建报价单请求，则原报价单将已处于 `canceled` 状态，本请求将返回错误 `70400`。当请求通过不同连接或进程发出时可能发生此情况，因为不同连接间不保证请求的处理顺序。如需确保严格的创建→取消顺序，请在收到创建报价单的响应后，通过同一连接再发出取消请求。

#### 限速: 50次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/cancel-quote`

> 请求示例

```shell
POST /api/v5/rfq/cancel-quote
{
    "quoteId": "007",
    "clQuoteId":"Bond007"
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

# 取消报价单
result = blockTradingAPI.cancel_quote(
    quoteId="007",
    clQuoteId="Bond007"
)
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                                                                |
| :------ | :----- | :----- |:----------------------------------------------------------------------------------|
| quoteId   |String |可选 | 报价单ID                                                                             |
| clQuoteId |String |可选 | 报价单自定义ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间，当 clRfqId 和 rfqId 都传时，以 rfqId 为准。 |
| rfqId     |String |否 | 询价单ID                                                                               |

> 返回示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "quoteId":"007",
            "clQuoteId":"Bond007",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                              |
| :------ | :----- |:------------------------------------------------|
| code	    |String	            | 结果代码，0 表示成功。                                    |
| msg	    |String	            | 错误信息，如果代码不为 0，则不为空。                             |
| data	    |Array of objects   | 包含结果的对象数组                                       |
| > quoteId	|String	            | 报价单ID                                           |
| > clQuoteId	|String	        | 询价单自定义ID                                        |
| > sCode	|String	            | 事件执行结果的code，0代表成功|
| > sMsg	|String	            | 事件执行失败时的msg          |
