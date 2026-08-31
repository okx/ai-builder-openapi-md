---
site: okex
lang: zh
name: 获取大宗交易单个产品行情信息
method: GET
path: /api/v5/market/block-ticker
module: market
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取大宗交易单个产品行情信息 [block-trading-rest-api-get-block-ticker]

获取最近24小时大宗交易量

#### 限速：20次/2s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/market/block-ticker`

> 请求示例

```shell
GET /api/v5/market/block-ticker?instId=BTC-USD-SWAP

```

```python
import okx.MarketData as MarketData

flag = "0"  # 实盘:0 , 模拟盘：1

marketDataAPI =  MarketData.MarketAPI(flag=flag)

# 获取大宗交易单个产品行情信息
result = marketDataAPI.get_block_ticker(
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                          |
| :----- | :----- | :------- | :---------------------------- |
| instId | String | 是       | 产品ID，如 `BTC-USD-SWAP` |

> 返回结果

```json
{
    "code":"0",
    "msg":"",
    "data":[
     {
        "instType":"SWAP",
        "instId":"LTC-USD-SWAP",
        "volCcy24h":"2222",
        "vol24h":"2222",
        "ts":"1597026383085"
     }
  ]
}
```

#### 返回参数

| **参数名** | **类型** | **描述**                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| instId     | String   | 产品ID                                                       |
| instType   | String   | 产品类型                                                     |
| volCcy24h  | String   | 24小时成交量，以`币`为单位<br>如果是`衍生品`合约，数值为交易货币的数量。<br>如果是`币币/币币杠杆`，数值为计价货币的数量。          |
| vol24h     | String   | 24小时成交量，以`张`为单位<br>如果是`衍生品`合约，数值为合约的张数。<br>如果是`币币/币币杠杆`，数值为交易货币的数量。          |
| ts         | String   | 数据产生时间，Unix时间戳的毫秒数格式，如 `1597026383085`     |
