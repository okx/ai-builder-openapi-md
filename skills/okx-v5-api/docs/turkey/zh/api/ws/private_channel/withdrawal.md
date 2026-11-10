---
site: turkey
lang: zh
name: 提币信息频道
type: guide
---

### 提币信息频道 [funding-account-websocket-withdrawal-info-channel]

当发起提币或者提币状态发生变化时会触发消息推送。<br>
支持母账户或者子账户的订阅<br>

- 如果是母账户订阅，可以同时接受母账户与子账户的提币信息的推送  
- 如果是子账户订阅，则仅支持子账户提币信息的推送  

#### 服务地址

/ws/v5/business (需要登录)

> 请求示例

```shell
{
  "id": "1512",
    "op": "subscribe",
    "args": [
        {
            "channel": "withdrawal-info"
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
        url = "wss://ws.okx.com/ws/v5/business",
        useServerTime=False
    )
    await ws.start()
    args = [
        {
            "channel": "withdrawal-info"
        }
    ]

    await ws.subscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

    await ws.unsubscribe(args, callback=callbackFunc)
    await asyncio.sleep(10)

asyncio.run(main())

```

#### 请求参数

| 参数名 | 类型   | 是否必填 | 描述                                 |
| :-------- | :----- |:-----| ------------------------------------------- |
| id     | String | 否       | 消息的唯一标识。<br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op        | String | 是    | 操作<br>`subscribe`<br>`unsubscribe`<br>|
| args      | Array of objects  | 是    | 请求订阅的频道列表         |
| > channel | String | 是    | 频道名<br>`withdrawal-info`  |
| > ccy | String | 否    | 币种名称，如 `BTC`   |

> 成功返回示例

```json
{
    "id": "1512",
    "event": "subscribe",
    "arg": {
        "channel": "withdrawal-info"
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
	"msg": "Invalid request: {\"op\": \"subscribe\", \"argss\":[{ \"channel\" : \"withdrawal-info\"}]}",
	"connId": "a4d3ae55"
}
```

#### 返回参数

| 参数名       | 类型   | 是否必填   | 描述                                  |
|:----------| :----- |:-------| -------------------------------------------- |
| id     | String | 否       | 消息的唯一标识 |
| event     | String | 是      | 操作<br>`subscribe`<br>`unsubscribe`<br>`error` |
| arg       | Object | 否      | 订阅的频道                           |
| > channel | String | 是      | 频道名<br>`withdrawal-info`           |
| > ccy     | String | 否      | 币种名称，如 `BTC`   |
| code      | String | 否      | 错误码                                   |
| msg       | String | 否      | 错误消息                                |
| connId   | String   | 是  | WebSocket连接ID |

> 推送示例

```json
{
	"arg": {
		"channel": "withdrawal-info",
		"uid": "289320*****0975104"
	},
	"data": [{
		"addrEx": null,
		"amt": "2",
		"areaCodeFrom": "",
		"areaCodeTo": "",
		"ccy": "USDT",
		"chain": "USDT-TRC20",
		"clientId": "",
		"fee": "0.8",
        "feeCcy": "USDT",
		"from": "",
		"memo": "",
        "nonTradableAsset": false,
        "note": "",
		"pTime": "1674103268578",
		"pmtId": "",
		"state": "0",
		"subAcct": "test",
		"tag": "",
		"to": "TN8CKTQMnpWfT******8KipbJ24ErguhF",
        "toAddrType": "1",
		"ts": "1674103268472",
		"txId": "",
		"uid": "289333*****1101696",
		"wdId": "63754560"
	}]
}
```

#### 推送数据参数

| **参数名**            | **类型** | **描述**                                                                                                                                                                                                                                                                                                    |
|--------------------| --------- |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| arg                | Object   | 订阅成功的频道                                                                                                                                                                                                                                                                                                   |
| > channel          | String   | 频道名                                                                                                                                                                                                                                                                                                       |
| > uid              | String   | 用户标识                                                                                                                                                                                                                                                                                                      |
| > ccy              | String | 币种名称，如 `BTC`                                                                                                                                                                                                                                                                                              |
| data               | Array of objects    | 订阅的数据                                                                                                                                                                                                                                                                                                     |
| > uid	             | String   | (产生数据者的）用户标识                                                                                                                                                                                                                                                                                              |
| > subAcct	         | String   | 子账户名称<br>如果是母账户产生的数据，该字段返回""                                                                                                                                                                                                                                                                              |
| > pTime	           | String   | 推送时间，Unix时间戳的毫秒数格式，如 1597026383085                                                                                                                                                                                                                                                                        |
| > ccy              | String  | 币种                                                                                                                                                                                                                                                                                                        |
| > chain            | String  | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链                                                                                                                                                                                                                                           |
| > nonTradableAsset | String  | 是否为不可交易资产<br>`true`：不可交易资产，`false`：可交易资产                                                                                                                                                                                                                                                                  |
| > amt              | String  | 数量                                                                                                                                                                                                                                                                                                        |
| > ts               | String  | 提币申请时间，Unix 时间戳的毫秒数格式，如 `1655251200000`                                                                                                                                                                                                                                                                   |
| > from             | String  | 提币账户<br>可以是`邮箱`/`手机号`/`子账户名`                                                                                                                                                                                                                                                                              |
| > areaCodeFrom     | String  | 如果`from`为手机号，该字段为该手机号的区号                                                                                                                                                                                                                                                                                  |
| > to               | String  | 收币地址                                                                                                                                                                                                                                                                                                      |
| > areaCodeTo       | String  | 如果`to`为手机号，该字段为该手机号的区号                                                                                                                                                                                                                                                                                    |
| > toAddrType       | String  | 地址类型<br>`1`: 钱包地址、邮箱、手机号、登录账户名<br>`2`: UID                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| > tag              | String  | 部分币种提币需要标签                                                                                                                                                                                                                                                                                                |
| > pmtId            | String  | 部分币种提币需要此字段（payment_id）                                                                                                                                                                                                                                                                                   |
| > memo             | String  | 部分币种提币需要此字段                                                                                                                                                                                                                                                                                               |
| > addrEx           | Object  | 提币地址备注。如币种TONCOIN的提币地址备注标签名为comment,则该字段返回：{'comment':'123456'}                                                                                                                                                                                                                                           |
| > txId             | String  | 提币哈希记录<br>内部转账该字段返回""                                                                                                                                                                                                                                                                                     |
| > fee              | String  | 提币手续费数量                                                                                                                                                                                                                                                                                                   |
| > feeCcy           | String  | 提币手续费币种，如 `USDT`                                                                                                                                                                                                                                                                                          |
| > state            | String  | 提币状态<br><br><li>阶段1：等待提币</li>`17`：钱包地址正等待国际转账规则认证<br>`10`：等待划转<br>`0`：等待提币<br>`4`/`5`/`6`/`8`/`9`/`12`：等待客服审核<br>`7`：审核通过<br><br><li>阶段2：提币处理中（适用于链上提币，内部转账无此阶段）</li>`1`：正在将您的交易广播到链上<br>`15`：交易待确认<br>`16`：根据当地法律法规，您的提币最多可能需要 24 小时才能到账<br>`-3`：撤销中<br><br><li>最终阶段</li>`-2`：已撤销<br>`-1`：失败<br>`2`：提币成功 |
| > wdId             | String  | 提币申请ID                                                                                                                                                                                                                                                                                                    |
| > clientId         | String  | 客户自定义ID                                                                                                                                                                                                                                                                                                   |
| > note             | String  | 备注信息                                                                                                                                                                                                                                                                                                      |
