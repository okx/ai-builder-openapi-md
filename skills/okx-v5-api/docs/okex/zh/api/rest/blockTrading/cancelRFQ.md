---
site: okex
lang: zh
name: 取消询价单
method: POST
path: /api/v5/rfq/cancel-rfq
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com
supported: true
---

### 取消询价单 [block-trading-rest-api-cancel-rfq]

取消一个询价单。

#### 限速: 5次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/cancel-rfq`

> 请求示例

```shell
POST /api/v5/rfq/cancel-rfq
{
    "rfqId":"22535",
    "clRfqId":"rfq001"
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

# 取消询价单
result = blockTradingAPI.cancel_rfq(
    rfqId="22535",
    clRfqId="rfq001"
)
print(result)
```

#### 请求参数
| 参数名  | 类型  | 是否必须    | 描述                                              |
| :------ | :----- | :----- | :-----------------------------------------------------------  |
| rfqId   |String |可选 |询价单ID|
| clRfqId |String |可选 |询价单自定义ID，字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。<br>当 clRfqId 和 rfqId 都传时，以 rfqId 为准。 |

> 返回示例

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "rfqId":"22535",
            "clRfqId":"rfq001",
            "sCode":"0",
            "sMsg":""
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                              |
| :------ | :----- | :-----------------------------------------------------------  |
| code	    |String	            |结果代码，0 表示成功。|
| msg	    |String	            |错误信息，如果代码不为 0，则不为空。|
| data	    |Array of objects   |包含结果的对象数组           |
| > rfqId	|String	            |RFQ ID                                             |
| > clRfqId	|String	            |由用户设置的 RFQ ID                            |
| > sCode	|String	            | 事件执行结果的code，0代表成功|
| > sMsg	|String	            | 事件执行失败时的msg          |
