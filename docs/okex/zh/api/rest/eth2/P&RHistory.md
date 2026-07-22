---
site: okex
lang: zh
name: GET / 获取申购赎回记录
method: GET
path: /api/v5/finance/staking-defi/eth/purchase-redeem-history
module: finance
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### GET / 获取申购赎回记录 [financial-product-eth-staking-get-purchase-amp-redeem-history]

#### 限速：6 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/finance/staking-defi/eth/purchase-redeem-history`

> 请求示例

```shell
GET /api/v5/finance/staking-defi/eth/purchase-redeem-history

```

```python
import okx.Finance.EthStaking as EthStaking

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

StackingAPI = EthStaking.EthStakingAPI(apikey, secretkey, passphrase, False, flag)

result = StackingAPI.eth_purchase_redeem_history()
print(result)
```

#### 请求参数

| 参数        | 类型   | 是否必须 | 描述                                                                    |
|:----------| :----- |:-----|:----------------------------------------------------------------------|
| type      | String | 否    | 类型<br>`purchase`：申购<br>`redeem`：赎回                                    |
| status    | String | 否    | 状态<br>`pending`：处理中<br>`success`：成功处理<br>`failed`：处理失败<br>`cancelled`：已取消                |
| after     | String | 否    | 请求此`requestTime`之前（更旧的数据）的分页内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| before    | String | 否    | 请求此`requestTime`之后（更新的数据）的分页内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| limit     | String | 否    | 返回结果的数量，默认100条，最大值为100条                                                |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "amt": "0.62666630",
            "completedTime": "1683413171000",
            "estCompletedTime": "",
            "redeemingAmt": "",
            "requestTime": "1683413171000",
            "status": "success",
            "type": "purchase"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| 参数名                | 类型     | 描述                                                 |
|--------------------|--------|----------------------------------------------------|
| type               | String | 类型<br>`purchase`：申购<br>`redeem`：赎回                 |
| amt                | String | 申购/赎回 的数量                                          |
| redeemingAmt       | String | 赎回中的数量                                             |
| status             | String | 状态<br>`pending`：处理中<br>`success`：成功处理<br>`failed`：处理失败<br>`cancelled`：已取消 |
| ordId                | String | 订单ID                                                                                             |
| requestTime        | String | 发起 申购/赎回 请求的时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085` |
| completedTime      | String | 赎回请求处理完成的时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085`  |
| estCompletedTime   | String | 预估完成赎回的时间，值为时间戳，Unix时间戳为毫秒数格式，如 `1597026383085`    |
