---
site: okex
lang: zh
name: 重设MMP状态
method: POST
path: /api/v5/rfq/mmp-reset
module: rfq
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 重设MMP状态 [block-trading-rest-api-reset-mmp-status]

重设MMP状态为无效。

#### 限速: 5次/2s
#### 限速规则：User ID
#### HTTP Requests

`POST /api/v5/rfq/mmp-reset`

> 请求示例

```shell
POST /api/v5/rfq/mmp-reset

```

```python
import okx.BlockTrading as BlockTrading

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘：1

blockTradingAPI = BlockTrading.BlockTradingAPI(apikey, secretkey, passphrase, False, flag)

# 重设MMP状态
result = blockTradingAPI.reset_mmp()
print(result)
```

#### 请求参数
None

> 

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "ts":"1597026383085"
        }
    ]
}
```

####  返回参数

| 参数名   | 类型   | 描述                                              |
|:------| :----- | :-----------------------------------------------------------  |
| code	 | String|   结果代码，`0` 表示成功                |
| msg	  | String|   错误信息，如果代码不为`0`，则不为空 |
| data	 | Array of objects|   请求返回值，包含请求结果 |
| > ts  | String| 重设时间. Unix 时间戳的毫秒数格式，如 `1597026383085`. |
