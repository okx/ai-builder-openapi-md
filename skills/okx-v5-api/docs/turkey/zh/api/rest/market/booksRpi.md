---
site: turkey
lang: zh
name: GET / 获取 RPI 产品深度
method: GET
path: /api/v5/market/books-rpi
module: market
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

### GET / 获取 RPI 产品深度 [order-book-trading-market-data-get-rpi-order-book]

获取产品的合并深度列表，在每个价格档位上将有机深度与当前可成交的 RPI（Retail Price Improvement，散户价格优化）深度合并返回。不可成交的 RPI 订单由平台侧过滤，不会返回。<br>
数据每 200 毫秒更新一次。该接口收到请求后不会立刻返回，而是会待服务端缓存数据更新后立即返回最新数据。

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/books-rpi`

> 请求示例

```shell
GET /api/v5/market/books-rpi?instId=BTC-USDT-SWAP&sz=3
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                           |
| :----- | :----- | :------- | :--------------------------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USDT-SWAP` |
| sz     | String | 否       | 深度档位数量，最大值可传400，即买卖深度共800条 <br>不填写此参数，默认返回`1`档深度数据|

> 返回结果

```json
{
    "code": "0",
    "msg": "",
    "data": [
        {
            "asks": [
                [
                    "67855.2",
                    "0.5",
                    "0.5",
                    "1"
                ],
                [
                    "67856.0",
                    "1.3",
                    "1.0",
                    "4"
                ],
                [
                    "67860.5",
                    "0.3",
                    "0",
                    "1"
                ]
            ],
            "bids": [
                [
                    "67854.8",
                    "1.7",
                    "1.2",
                    "3"
                ],
                [
                    "67853.0",
                    "0.8",
                    "0.8",
                    "1"
                ]
            ],
            "ts": "1785310731002",
            "seqId": 332042172451
        }
    ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**       |
| :--------- | :------- | :------------- |
| asks       | Array of Arrays | 卖方深度，每个元素为 `[price, totalQty, nonRpiQty, count]` |
| bids       | Array of Arrays | 买方深度，每个元素为 `[price, totalQty, nonRpiQty, count]` |
| ts         | String   | 深度产生的时间，Unix 时间戳，单位为毫秒 |
| seqId      | Integer  | 当前消息的序列号，与 `books-rpi` WebSocket 频道保持一致 |

<aside class="notice">
asks和bids值数组举例说明： ["67856.0", "1.3", "1.0", "4"]<br>
- 67856.0 为深度价格<br>
- 1.3 为 <code>totalQty</code>，即该价格的总数量，包含有机深度与当前可成交的 RPI 深度（合约交易为张数，现货/币币杠杆为交易币的数量）<br>
- 1.0 为 <code>nonRpiQty</code>，即该价格中有机（非 RPI）部分的数量<br>
- 4 为该价格的订单数量，包含有机订单与当前可成交的 RPI 订单<br>
<br>
该价格档位上可成交的 RPI 数量为 <code>totalQty - nonRpiQty</code>。具备 RPI 权限的 taker 可成交至 <code>totalQty</code>；不具备 RPI 权限的 taker 仅可成交至 <code>nonRpiQty</code>，即使二者读取同一份数据。下单时将 `rpiTakerAccess` 设为 `true` 即可使用 RPI 流动性。<br>
<br>
请注意，仅本接口的第三位为 <code>nonRpiQty</code>。在 `books`、`books-full`、`books-lite` 接口中，同一位置为已弃用字段，始终为 "0"。<br>
<br>
当某档位的 <code>totalQty</code> 与 <code>nonRpiQty</code> 相等时，表示该价格上当前没有可成交的 RPI 深度。对于没有 RPI 做市商报价的产品，以及依照撮合规则当前被隐藏的 RPI 挂单，出现该情况均属正常。
</aside>

<aside class="notice">
本接口不返回 <code>checksum</code>，请使用 <code>seqId</code> 进行排序校验。<br>
当 RPI 可成交状态不可用时，本接口以保守方式降级：排除 RPI 数量，每个档位返回的 <code>totalQty</code> 与 <code>nonRpiQty</code> 相等。
</aside>
