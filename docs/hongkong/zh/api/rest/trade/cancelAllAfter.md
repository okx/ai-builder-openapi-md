---
site: hongkong
lang: zh
name: POST / 倒计时全部撤单
method: POST
path: /api/v5/trade/cancel-all-after
module: trade
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### POST / 倒计时全部撤单[order-book-trading-trade-post-cancel-all-after]

在倒计时结束后，取消所有挂单。适用于所有撮合交易产品（不包括价差交易）。

#### 限速：1次/s
#### 限速规则：User ID + tag

#### HTTP请求

`POST /api/v5/trade/cancel-all-after`

> 请求示例

```shell
POST /api/v5/trade/cancel-all-after
{
   "timeOut":"60"
}
```

```python
import okx.Trade as Trade

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘: 0, 模拟盘: 1

tradeAPI = Trade.TradeAPI(apikey, secretkey, passphrase, False, flag)

# 设置倒计时全部撤单
result = tradeAPI.cancel_all_after(
    timeOut="10"
)

print(result)

```

#### 请求参数

| 参数名  | 类型   | 是否必须 | 描述                                                        |
| :------ | :----- |:-----|:----------------------------------------------------------|
| timeOut  | String | 是    | 取消挂单的倒计时，单位为秒<br>取值范围为 0, [10, 120]<br>0 代表不使用该功能 |
| tag | String | 否 | CAA订单标签<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字，且长度在1-16位之间 |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
        {
            "triggerTime":"1587971460",
            "tag":"",
            "ts":"1587971400"
        }
    ]
}
```

#### 返回参数

| 参数名  | 类型   | 描述                                        |
| :------ | :----- |:------------------------------------------|
| triggerTime     | String | 触发撤单的时间<br>triggerTime=0 代表未使用该功能         |
| tag     | String | CAA订单标签                                   |
| ts     | String | 请求被接收到的时间                                 |

<aside class="notice">
建议用户每一秒调用接口一次。当倒计时全部撤单被触发时，交易引擎将为用户逐一取消其挂单，该操作可能持续数秒。该功能起到保护用户的作用，不应作为交易策略使用。
</aside>

<aside class="notice">
<br>
为使用标签维度倒计时全部撤单，首先，用户需使用现有下单接口的tag请求参数，为订单设置标签。调用CAA接口时，若不传入tag请求参数，则默认设置账户维度CAA，CAA触发时，撤销该子账户下的所有撮合交易产品挂单；若传入tag请求参数，则默认设置订单标签维度CAA，CAA触发时，带有此tag的撮合交易产品挂单将被撤销，带有其他tag或没有tag的订单将不受影响。
<br>
<br>
同一子账户下，用户最多能同时运行20个标签维度的CAA。系统仅计数活跃的标签维度CAA，已被触发或被用户主动撤销的将不被计入。超过限制时，用户将收到错误码51071。
</aside>
