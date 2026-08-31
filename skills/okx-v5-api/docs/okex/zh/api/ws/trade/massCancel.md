---
site: okex
lang: zh
name: WS / 撤销 MMP 订单
type: guide
---

### WS / 撤销 MMP 订单 [order-book-trading-trade-ws-mass-cancel-order]

撤销同一交易品种下用户所有的 MMP 挂单<br>
仅适用于组合保证金账户模式下的期权订单，且有 MMP 权限。

#### 服务地址

/ws/v5/private (需要登录)

#### 限速：5次/2s 
#### 限速规则：User ID

<aside class="notice">
同`撤销 MMP 订单` REST API 共享限速
</aside>

> 请求示例

```shell
{
    "id": "1512",
    "op": "mass-cancel",
    "args": [{
        "instType":"OPTION",
        "instFamily":"BTC-USD"
    }]
}
```

#### 请求参数

| 参数名          | 类型   | 是否必须 | 描述                                                         |
|:-------------| :----- | :------- | :----------------------------------------------------------- |
| id           | String | 是       | 消息的唯一标识 <br>用户提供，返回参数中会返回以便于找到相应的请求。<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度必须要在1-32位之间。 |
| op           | String | 是       | 支持的业务操作，如  `mass-cancel`                               |
| args         | Array of objects  | 是       | 请求参数                                                     |
| > instType   | String | 是    | 交易产品类型<br>`OPTION`:期权                                                                                       |
| > instFamily | String | 是    | 交易品种                                                                                                         |
| > lockInterval | String | 否 | 锁定时长(毫秒)<br> 范围应为[0, 10 000]<br> 默认为 0. 如果想要立即解锁，您可以设置为 "0"<br> 下单时，如果在该锁定期间，会报错 54008，如果在 MMP 触发期间，会报错 51034 |

> 成功返回示例

```json
{
    "id": "1512",
    "op": "mass-cancel",
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
	"op": "mass-cancel",
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
