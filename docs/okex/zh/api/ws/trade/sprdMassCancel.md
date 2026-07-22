---
site: okex
lang: zh
name: WS / 全部撤单
type: guide
---

### WS / 全部撤单 [spread-trading-websocket-trade-api-ws-cancel-all-orders]

#### 服务地址

/ws/v5/business (需要登录)

#### 限速：5次/2s 
#### 限速规则：User ID

> 请求示例

```shell
{
    "id": "1512",
    "op": "sprd-mass-cancel",
    "args": [{
        "sprdId":"BTC-USDT_BTC-USDT-SWAP"
    }]
}
```

#### 请求参数

| 参数名          | 类型   | 是否必须 | 描述                                                         |
|:-------------| :----- | :------- | :----------------------------------------------------------- |
| id           | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op           | String | 是       | 支持的业务操作，`sprd-mass-cancel`                               |
| args         | Array of objects  | 是       | 请求参数                                                     |
| > sprdId | String | 否    | 价差ID                                                                                                        |

> 成功返回示例

```json
{
    "id": "1512",
    "op": "sprd-mass-cancel",
    "data": [
        {
            "result": true
        }
    ],
    "code": "0",
    "msg": ""
} 
```

> 格式错误返回示例

```json
{
	"id": "1512",
	"op": "sprd-mass-cancel",
	"data": [],
	"code": "60013",
	"msg": "Invalid args"
}
```

#### 返回参数

| 参数名      | 类型   | 描述             |
|:---------| :----- | :--------------- |
| id       | String | 消息的唯一标识   |
| op       | String | 业务操作   |
| code     | String | 代码             |
| msg      | String | 消息             |
| data     | Array of objects  | 请求成功后返回的数据 |
| > result | Boolean | 撤单结果<br>`true`：全部撤单成功<br>`false`：全部撤单失败 |
