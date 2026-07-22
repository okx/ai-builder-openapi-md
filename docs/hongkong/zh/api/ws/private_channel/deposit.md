---
site: hongkong
lang: zh
name: WebSocket
type: guide
---

## WebSocket [funding-account-websocket]
### 充值信息频道 [funding-account-websocket-deposit-info-channel]

当发起充值或者充值状态发生变化时会触发消息推送。<br>
支持母账户或者子账户的订阅  <br>

- 如果是母账户订阅，可以同时接受母账户与子账户的充值信息的推送  
- 如果是子账户订阅，则仅支持子账户充值信息的推送  

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
    "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "deposit-info"
        }
    ]
}
```

```python
import asyncio
from okx.websocket.WsPrivateAsync import WsPrivateAsync

def callbackFunc(message):
    print(message)

async def main():
    ws = WsPrivateAsync(
        apiKey = "YOUR_API_KEY",
        passphrase = "YOUR_PASSPHRASE",
        secretKey = "YOUR_SECRET_KEY",
        url = "wss://ws.okx.com:8443/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "deposit-info"
        }
    ]
    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数名         | 类型               | 是否必填 | 描述                                 |
|:------------|:-----------------|:-----| ------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op          | String           | 是    | 操作<br>`subscribe`<br>`unsubscribe`<br>|
| args        | Array of objects | 是    | 请求订阅的频道列表         |
| > channel   | String           | 是    | 频道名<br>`deposit-info`  |
| > ccy       | String           | 否    | 币种名称，如 `BTC`   |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "deposit-info"
    },
	"connId": "a4d3ae55"
}
```

> 失败返回示例

```json
{
    "id": "1512",
    "event": "error",
    "code": "60012",
    "msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"deposit-info\""}]}",
	"connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名       | 类型   | 是否必填   | 描述                                  |
|:----------| :----- |:-------| -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是      | 操作<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | 否      | 订阅的频道                           |
| > channel | String | 是      | 频道名<br>`deposit-info`           |
| > ccy     | String | 否      | 币种名称，如 `BTC`   |
| code      | String | 否      | 错误码                                   |
| msg       | String | 否      | 错误消息                                |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "deposit-info",
		"uid": "289320****60975104"
	},
	"data": [{
		"actualDepBlkConfirm": "0",
		"amt": "1",
		"areaCodeFrom": "",
		"ccy": "USDT",
		"chain": "USDT-TRC20",
		"depId": "88165462",
		"from": "",
        "fromWdId": "",
		"pTime": "1674103661147",
		"state": "0",
		"subAcct": "test",
		"to": "TEhFAqpuHa3LY*****8ByNoGnrmexeGMw",
		"ts": "1674103661123",
		"txId": "bc5376817*****************dbb0d729f6b",
		"uid": "289320****60975104"
	}]
}
```

#### 推送数据参数

| **参数名**               | **类型** | **描述**                                                                                                                                   |
|-----------------------|-------|------------------------------------------------------------------------------------------------------------------------------------------|
| arg                   | Object | 订阅成功的频道                                                                                                                                  |
| > channel             | String | 频道名<br>`deposit-info`                                                                                                                                      |
| > uid                 | String | 用户标识                                                                                                                                     |
| > ccy                 | String | 币种名称，如 `BTC`                                                                                                                             |
| data                  | Array of objects | 订阅的数据                                                                                                                                    |
| > uid	                | String | (产生数据者的）用户标识                                                                                                                             |
| > subAcct	            | String | 子账户名称<br>如果是母账户产生的数据，该字段返回""                                                                                                             |
| > pTime	              | String | 推送时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                       |
| > ccy                 | String | 币种名称，如 `BTC`                                                                                                                             |
| > chain               | String | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链                                                                          |
| > amt                 | String | 充值数量                                                                                                                                     |
| > from                | String | 充值账户，仅展示内部账户的转账地址（手机号和邮箱将做脱敏处理），不展示区块链充值地址                                                                                                              |
| > areaCodeFrom        | String | 如果`from`为手机号，该字段为该手机号的区号                                                                                                                 |
| > to                  | String | 到账地址                                                                                                                                     |
| > txId                | String | 区块转账哈希记录                                                                                                                                 |
| > ts                  | String | 充值记录创建时间，Unix 时间戳的毫秒数格式，如 `1655251200000`                                                                                                |
| > state               | String | 充值状态<br>`0`：等待确认 <br>`1`：确认到账 <br>`2`：充值成功 <br>`8`：因该币种暂停充值而未到账，恢复充值后自动到账<br>`11`：命中地址黑名单<br>`12`：账户或充值被冻结<br>`13`：子账户充值拦截<br>`14`：KYC限额 |
| > depId               | String | 充值记录 ID                                                                                                                                  |
| > fromWdId            | String | 内部转账发起者提币申请 ID<br>如果该笔充值来自于内部转账，则该字段展示内部转账发起者的提币申请 ID，其他情况返回""                                                                           |
| > actualDepBlkConfirm | String | 最新的充币网络确认数                                                                                                                               |
