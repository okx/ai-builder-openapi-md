---
site: okex
lang: zh
name: Websocket交易API
type: guide
---

## Websocket交易API
### WS / 下单 [spread-trading-websocket-trade-api-ws-place-order]

只有当您的账户有足够的资金才能下单。

#### 服务地址

/ws/v5/business (需要登录)

#### 限速：20次/2s 
#### 限速规则：User ID

<aside class="notice">
同Nitro Spread`下单` REST API 共享限速
</aside>

> 请求示例

```shell
{
  "id": "1512",
  "op": "sprd-order",
  "args": [
    {
       "sprdId":"BTC-USDT_BTC-USDT-SWAP",
       "clOrdId":"b15",
       "side":"buy",
       "ordType":"limit",
       "px":"2.15",
       "sz":"2"
    }
  ]
}
```

#### 请求参数

| 参数名            | 类型   | 是否必须 | 描述                                                                                                                                                                                                                                                      |
|:---------------| :----- | :------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id             | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。                                                                                                                                                                  |
| op             | String | 是       | 支持的业务操作，`sprd-order`                                                                                                                                                                                                                                      |
| args           | Array of objects  | 是       | 请求参数                                                                                                                                                                                                                                                    |
| > sprdId       | String  | 是       | 产品ID，如 `BTC-USDT_BTC-USDT-SWAP`                                                                                                                                                                                                                          |
| > clOrdId      | String  | 否       | 由用户设置的订单ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                                                                                |
| > tag          | String  | 否       | 订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-16位之间。                                                                                                                                                                                                       |
| > side         | String  | 是       | 订单方向，`buy`   `sell`                                                                                                                                                                                                                                     |
| > ordType      | String  | 是       | 订单类型<br>`market`：市价单  <br>`limit`：限价单 <br>`post_only`：只做maker单 <br>`ioc`：立即成交并取消剩余|
| > sz           | String  | 是       | 委托数量                                                                                                                                                                                                                                                    |
| > px           | String  | 是       | 委托价，仅适用于`limit`、`post_only`、`ioc`类型的订单                                                                                                                                                                                                     |

> 成功返回示例

```json
{
	"id": "1512",
	"op": "sprd-order",
	"data": [{
		"clOrdId": "",
		"ordId": "12345689",
		"tag": "",
		"sCode": "0",
		"sMsg": ""
	}],
	"code": "0",
	"msg": ""
}
```

> 失败返回示例

```json
{
	"id": "1512",
	"op": "sprd-order",
	"data": [{
		"clOrdId": "",
		"ordId": "",
		"tag": "",
		"sCode": "5XXXX",
		"sMsg": "not exist"
	}],
	"code": "1",
	"msg": ""
}
```

> 格式错误返回示例

```json
{
	"id": "1512",
	"op": "sprd-order",
	"data": [],
	"code": "60013",
	"msg": "Invalid args"
}
```

#### 返回参数

| 参数名 | 类型   | 描述             |
| :----- | :----- | :--------------- |
| id     | String | 消息的唯一标识   |
| op     | String | 业务操作   |
| code   | String | 代码             |
| msg    | String | 消息             |
| data   | Array of objects  | 请求成功后返回的数据 |
| > ordId     | String   | 订单ID                                |
| > clOrdId   | String   | 由用户设置的订单ID      |
| > tag       | String   | 订单标签                              |
| > sCode     | String   | 订单状态码，0 代表成功 |
| > sMsg      | String   | 订单状态消息             |

<aside class="notice">
clOrdId<br>
clOrdId是用户自定义的唯一ID用来识别订单。如果在请求参数中传入了，那它一定会在返回参数内，并且可以用于查询订单，撤销订单，修改订单等接口。
clOrdId不能与当前所有的挂单的clOrdId重复
</aside>
