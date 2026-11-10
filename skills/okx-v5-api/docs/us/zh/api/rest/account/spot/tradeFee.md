---
site: us
lang: zh
name: 获取当前账户交易手续费费率
method: GET
path: /api/v5/account/trade-fee
module: account
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### 获取当前账户交易手续费费率 [trading-account-rest-api-get-fee-rates]

#### 限速：5次/2s
#### 限速规则：User ID

#### HTTP请求

`GET /api/v5/account/trade-fee`

> 请求示例

```shell
# 获取币币BTC-USDT交易手续费率  
GET /api/v5/account/trade-fee?instType=SPOT&instId=BTC-USDT
```

```python
import okx.Account as Account

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "1"  # 实盘:0 , 模拟盘:1

accountAPI = Account.AccountAPI(apikey, secretkey, passphrase, False, flag)

# 获取当前账户交易手续费费率
result = accountAPI.get_fee_rates(
    instType="SPOT",
    instId="BTC-USDT"
)
print(result)
```

#### 请求参数

| 参数名        | 类型   | 是否必须   | 描述                                      |
|:-----------| :----- |:-------|:----------------------------------------|
| instType   | String | 是      | 产品类型<br>`SPOT`：币币                       |
| instId     | String | 否      | 产品ID，如 `BTC-USDT`<br>仅适用于instType为`币币` | 
| groupId             | String         | 否    | 交易产品手续费分组ID<br> groupId 和 instId 只能传入其一<br><br> 用户可以使用交易产品基础信息接口获取产品ID及其手续费分组ID的对应关系                                            |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "category": "1",
            "delivery": "",
            "exercise": "",
            "feeGroup": [
                {
                    "groupId": "1",
                    "maker": "-0.0008",
                    "taker": "-0.001"
                }
            ],
            "fiat": [],
            "instType": "SPOT",
            "level": "Lv1",
            "maker": "-0.0008",
            "makerU": "",
            "makerUSDC": "",
            "ruleType": "normal",
            "taker": "-0.001",
            "takerU": "",
            "takerUSDC": "",
            "ts": "1763979985847"
        }
    ],
    "msg": ""
}
```

#### 返回参数

| **参数名**    | **类型**     | **描述**                                              |
|------------|------------|-----------------------------------------------------|
| level      | String     | 手续费等级                                               |
| feeGroup | Array of objects | 手续费分组 <br>适用于`SPOT/MARGIN/SWAP/FUTURES/OPTION` |
| > taker | String | 吃单手续费 |
| > maker | String | 挂单手续费 |
| > groupId | String | 交易产品手续费分组ID<br><br>**用户需要同时使用instType和groupId来确定一个交易产品的交易手续费分组；用户应该将此接口和[获取交易产品基础信息](/docs-v5/zh/#trading-account-rest-api-get-instruments)一起使用，以获取特定交易产品的手续费率**|
| delivery   | String     | 交割手续费率                                              |
| exercise   | String     | 行权手续费率                                              |
| instType   | String     | 产品类型                                                |
| ts         | String     | 数据返回时间，Unix时间戳的毫秒数格式，如 `1597026383085`              |
| taker      | String     | ~~对于`币币`，为 USDT 交易区的吃单手续费率~~（已废弃） |
| maker      | String     | ~~对于`币币`，为 USDT 交易区的挂单手续费率~~（已废弃） |
| takerU     | String     | ~~USDT 合约吃单手续费率，仅适用于`交割/永续`~~（已废弃）                           |
| makerU     | String     | ~~USDT 合约挂单手续费率，仅适用于`交割/永续`~~（已废弃）                           |
| takerUSDC  | String     | ~~对于币币，为 USDⓈ&Crypto 交易区的吃单手续费率~~（已废弃） |
| makerUSDC  | String     | ~~对于币币，为 USDⓈ&Crypto 交易区的挂单手续费率~~（已废弃）     |
| category   | String     | ~~币种类别~~（已废弃）                                 |
| fiat       | Array of objects      | ~~法币费率~~（已废弃）                                                           |
| > ccy      | String     | 法币币种                                                           |
| > taker    | String     | 吃单手续费率                                                         |
| > maker    | String     | 挂单手续费率                                                         |

<aside class="notice">
备注：<br>
手续费率的值（如 maker/taker）：正数，代表是返佣的费率；负数，代表平台扣除的费率。
</aside>

<aside class="notice">
USDⓈ 代表除 USDT 之外的稳定币。
</aside>
