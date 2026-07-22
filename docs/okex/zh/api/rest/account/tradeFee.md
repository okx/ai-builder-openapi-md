---
site: okex
lang: zh
name: 获取当前账户交易手续费费率
method: GET
path: /api/v5/account/trade-fee
module: account
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
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

| 参数名            | 类型   | 是否必须 | 描述                                                                                           |
|:---------------| :----- |:-----|:---------------------------------------------------------------------------------------------|
| instType       | String | 是    | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`EVENTS`：事件合约 |
| instId         | String | 否    | 产品ID，如 `BTC-USDT`<br>仅适用于instType为`币币/币币杠杆`<br>指定此参数将返回正确的适用手续费率（如：参与做市激励计划用户的做市商费率）。                                                  |
| instFamily     | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`，如 `BTC-USD`                                                                         |                                                                                                                                 
| groupId             | String         | 否    | 交易产品手续费分组ID<br> groupId 和 instId/instFamily 只能传入其一<br><br> 用户可以使用交易产品基础信息接口获取产品ID及其手续费分组ID的对应关系                                            |

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
                    "elpMaker": "-0.0008",
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

| **参数名**  | **类型** | **描述**                                               |
|----------| -------- |------------------------------------------------------|
| level    | String   | 手续费等级                                                |
| feeGroup | Array of objects | 手续费分组 <br>适用于`SPOT/MARGIN/SWAP/FUTURES/OPTION/EVENTS` |
| > taker | String | 吃单手续费<br>`EVENTS` 吃单手续费公式的 K1 参数：`K1 × C × (P × (1-P))`（C = 合约张数，P = 价格） |
| > maker | String | 挂单手续费<br>`EVENTS` 挂单手续费公式的 K2 参数：`K2 × C × (P × (1-P))`（C = 合约张数，P = 价格） |
| > groupId | String | 交易产品手续费分组ID<br><br>**用户需要同时使用instType和groupId来确定一个交易产品的交易手续费分组；用户应该将此接口和[获取交易产品基础信息](/docs-v5/zh/#trading-account-rest-api-get-instruments)一起使用，以获取特定交易产品的手续费率**|
| > elpMaker | String | ELP Maker 有效费率。若 ELP 不适用于该交易产品，则返回 `""`。 |
| delivery | String   | 交割手续费率                                               |
| exercise | String   | 行权手续费率                                               |
| instType | String   | 产品类型                                                 |
| ts       | String   | 数据返回时间，Unix时间戳的毫秒数格式，如 `1597026383085`               |
| taker    | String | ~~对于币币/杠杆，为 USDT 交易区的吃单手续费率；<br>对于永续，交割和期权合约，为币本位合约费率~~（已废弃） |
| maker    | String | ~~对于币币/杠杆，为 USDT 交易区的挂单手续费率；<br>对于永续，交割和期权合约，为币本位合约费率~~（已废弃） |
| takerU   | String   | ~~USDT 合约吃单手续费率，仅适用于`交割/永续`~~（已废弃）                            |
| makerU   | String   | ~~USDT 合约挂单手续费率，仅适用于`交割/永续`~~（已废弃）                            |
| takerUSDC   | String | ~~对于币币/杠杆，为 USDⓈ&Crypto 交易区的吃单手续费率；<br>对于永续和交割合约，为 USDC 合约费率~~（已废弃） |
| makerUSDC   | String | ~~对于币币/杠杆，为 USDⓈ&Crypto 交易区的挂单手续费率；<br>对于永续和交割合约，为 USDC 合约费率~~（已废弃）      |
| ruleType | String | ~~交易规则类型<br> `normal`：普通交易<br> `pre_market`：盘前交易~~（已废弃） |
| category | String   | ~~币种类别~~（已废弃）                                  |
| fiat   | Array of objects  | ~~法币费率~~（已废弃）                                                            |
| > ccy   | String  | 法币币种                                                            |
| > taker   | String  | 吃单手续费率                                                          |
| > maker   | String  | 挂单手续费率                                                          |
| settle | String | 结算手续费率，适用于持仓方向与事件合约结算结果一致的用户。持反向仓位的用户结算时不收取手续费。仅适用于 `EVENTS` |

<aside class="notice">
备注：<br>
手续费率的值（如 maker/taker）：正数，代表是返佣的费率；负数，代表平台扣除的费率。<br>
例外：delivery 和 exercise 为正数，代表平台扣除的费率。
</aside>

<aside class="notice">
USDⓈ 代表除 USDT 之外的稳定币。
</aside>

<aside class="notice">
接口不会体现零手续费，零手续费交易对请参考<a href="https://www.okx.com/zh-hans/fees">https://www.okx.com/zh-hans/fees</a>
</aside>

<aside class="notice">
对于参与做市激励计划的用户：指定 <code>instId</code>（适用于 <code>SPOT</code>/<code>MARGIN</code>）或 <code>instFamily</code>（适用于 <code>FUTURES</code>/<code>SWAP</code>/<code>OPTION</code>）将返回正确的适用手续费率；若不指定上述参数，则返回基础档位手续费率。

</aside>
