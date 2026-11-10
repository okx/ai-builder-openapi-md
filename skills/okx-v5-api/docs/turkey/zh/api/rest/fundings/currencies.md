---
site: turkey
lang: zh
name: 获取币种列表
method: GET
path: /api/v5/asset/currencies
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com
supported: true
---

## REST API [funding-account-rest-api]
### 获取币种列表 [funding-account-rest-api-get-currencies]

获取当前用户KYC实体支持的币种列表。

#### 限速：6 次/s
#### 限速规则：User ID

#### HTTP 请求

`GET /api/v5/asset/currencies`

> 请求示例

```shell
GET /api/v5/asset/currencies

```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 获取币种列表
result = fundingAPI.get_currencies()
print(result)
```

#### 请求参数

| 参数 | 类型   | 是否必须 | 描述                    |
| :--- | :----- | :------- | :---------------------- |
| ccy  | String | 否       | 币种，如 `BTC`<br>支持多币种查询，币种之间半角逗号分隔 |

> 返回结果

```json
{
  "code": "0",
  "msg": "",
  "data": [
    {
        "burningFeeRate": "",
        "canDep": true,
        "canInternal": true,
        "canWd": true,
        "ccy": "BTC",
        "chain": "BTC-Bitcoin",
        "ctAddr": "",
        "depEstOpenTime": "",
        "depQuotaFixed": "",
        "depQuoteDailyLayer2": "",
        "fee": "0.00005",
        "logoLink": "https://static.coinall.ltd/cdn/oksupport/asset/currency/icon/btc20230419112752.png",
        "mainNet": true,
        "maxFee": "0.00005",
        "maxFeeForCtAddr": "",
        "maxWd": "500",
        "minDep": "0.0005",
        "minDepArrivalConfirm": "1",
        "minFee": "0.00005",
        "minFeeForCtAddr": "",
        "minInternal": "0.0001",
        "minWd": "0.0005",
        "minWdUnlockConfirm": "2",
        "name": "Bitcoin",
        "needTag": false,
        "usedDepQuotaFixed": "",
        "usedWdQuota": "0",
        "wdEstOpenTime": "",
        "wdQuota": "10000000",
        "wdTickSz": "8"
    }
  ]
}
```

#### 返回参数

| 参数名                  | 类型    | 描述                                                                                           |
|----------------------| ------- |----------------------------------------------------------------------------------------------|
| ccy                  | String  | 币种名称，如 `BTC`                                                                                 |
| name                 | String  | 币种名称，不显示则无对应名称                                                                               |
| logoLink             | String  | 币种Logo链接                                                                                     |
| chain                | String  | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链                              |
| ctAddr               | String  | 合约地址                                                                                         |
| canDep               | Boolean | 当前是否可充值<br>`false`：不可链上充值<br>`true`：可以链上充值                                                   |
| canWd                | Boolean | 当前是否可提币<br>`false`：不可链上提币<br>`true`：可以链上提币                                                   |
| canInternal          | Boolean | 当前是否可内部转账<br>`false`：不可内部转账<br>`true`：可以内部转账                                                 |
| depEstOpenTime       | String | 充值预期开放时间，Unix时间戳的毫秒数格式，如 `1597026383085`<br>如果 `canDep` 为 `true`，则返回 `""`                                                     |
| wdEstOpenTime        | String | 提币预期开放时间，Unix时间戳的毫秒数格式，如 `1597026383085`<br>如果 `canWd` 为 `true`，则返回 `""`                                                     |
| minDep               | String  | 币种单笔最小充值量                                                                                    |
| minWd                | String  | 币种单笔最小`链上提币`量                                                                                |
| minInternal          | String  | 币种单笔最小`内部转账`量<br>无单笔最大`内部转账`量限制，受24小时内提币额度(`wdQuota`)限制                                               |
| maxWd                | String  | 币种单笔最大`链上提币`量                                                                                |
| wdTickSz             | String  | 提币精度,表示小数点后的位数。提币手续费精度与提币精度保持一致。<br>内部转账提币精度为小数点后8位。                                         |
| wdQuota              | String  | 过去24小时内提币额度（包含`链上提币`和`内部转账`），单位为`USD`                                                        |
| usedWdQuota          | String  | 过去24小时内已用提币额度，单位为`USD`                                                                       |
| fee                  | String  | 固定的提币手续费数量<br>适用于`链上提币`                                                                      |
| minFee               | String  | <del>普通地址最小提币手续费数量<br>适用于`链上提币`</del><br>该字段已废弃                                              |
| maxFee               | String  | <del>普通地址最大提币手续费数量<br>适用于`链上提币`</del><br>该字段已废弃                                              |
| minFeeForCtAddr      | String  | <del>合约地址最小提币手续费数量<br>适用于`链上提币`</del><br>该字段已废弃                                              |
| maxFeeForCtAddr      | String  | <del>合约地址最大提币手续费数量<br>适用于`链上提币`</del><br>该字段已废弃                                              |
| burningFeeRate       | String  | 燃烧费率，如 `0.05` 代表 `5%`。<br>部分币种会收取燃烧费用。燃烧费用按照提币数量（不含gas fee） 乘以 燃烧费率，在提币数量基础上扣除。<br>适用于`链上提币` |
| mainNet              | Boolean  | 当前链是否为主链                                                                                     |
| needTag              | Boolean  | 当前链提币是否需要标签（tag/memo）信息，如 `EOS`该字段为`true`                                                    |
| minDepArrivalConfirm | String  | 充值到账最小网络确认数。币已到账但不可提。                                                                        |
| minWdUnlockConfirm   | String  | 提现解锁最小网络确认数                                                                                  |
| depQuotaFixed        | String  | 充币固定限额，单位为`USD`<br>没有充币限制则返回""                                                               |
| usedDepQuotaFixed    | String  | 已用充币固定额度，单位为`USD`<br>没有充币限制则返回""                                                             |
| depQuoteDailyLayer2  | String  | Layer2网络每日充值上限                                                                               |
