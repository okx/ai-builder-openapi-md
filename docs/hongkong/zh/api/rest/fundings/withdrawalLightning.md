---
site: hongkong
lang: zh
name: 闪电网络提币
method: POST
path: /api/v5/asset/withdrawal-lightning
module: asset
rest: https://www.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 闪电网络提币 [funding-account-rest-api-lightning-withdrawals]

单笔提币金额最大值为"0.1BTC"，最小值暂定为"0.000001BTC"，最近24小时内最大提币数量为"1BTC"。子账户不支持提币。

<aside class="notice">
仅针对部分用户开放此API功能服务，如果需要此功能服务请发邮件至`pablo.magro@okx.com`申请
</aside>

#### 限速：2次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/asset/withdrawal-lightning`

> 请求示例

```shell
POST /api/v5/asset/withdrawal-lightning
body
{
    "ccy":"BTC",
    "invoice":"lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hz"
}

```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 闪电网络提币
result = fundingAPI.withdrawal_lightning(
    ccy="BTC",
    invoice="lnbc100u1psnnvhtpp5yq2x3q5hhrzsuxpwx7ptphwzc4k4wk0j3stp0099968m44cyjg9sdqqcqzpgxqzjcsp5hz"
)
print(result)
```

#### 请求参数

| 参数名                      | 类型     | 是否必须   | 描述                                                                                                                                                              |
|:-------------------------|:-------|:-------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                      | String | 是      | 币种，如 `BTC`                                                                                                                                                      |
| invoice                  | String | 是      | invoice 号码                                                                                                                                                      |
| memo                     | String | 否      | 闪电网络提币的备注                                                                                                                                                       |
| rcvrInfo                 | Object | 可选      | 接收方信息<br>特定主体用户做`链上提币`/`闪电网络提币` 需要提供此信息                                                                                                                         |
| > walletType             | String | 是       | 钱包类型<br>`exchange`：提币到交易所钱包<br>`private`：提币到私人钱包<br>如果提币到交易所钱包，必须提供接收方相关信息<br>提币到私人钱包，则不需要提供接收方信息                                                               |
| > exchId                 | String | 可选      | 交易所 ID<br>可以通过 [获取交易所列表（公共）](/docs-v5/zh/#funding-account-rest-api-get-exchange-list-public) 接口查询支持的交易所<br>如果交易所不在支持的交易所列表中，该字段填`0`<br>适用于walletType=`exchange` |
| > rcvrFirstName          | String | 可选      | 接收方名字，如 `Bruce`<br>适用于walletType=`exchange`                                                                                                                     |
| > rcvrLastName           | String | 可选      | 接收方姓氏，如 `Wayne`<br>适用于walletType=`exchange`                                                                                                                     |
| > rcvrCountry            | String | 可选      | 接收方所在国家，如 `United States`<br>适用于walletType=`exchange`                                                                                                           |
| > rcvrCountrySubDivision | String | 可选      | 接收方所在州/省，如 `California`<br>适用于walletType=`exchange`                                                                                                             |
| > rcvrTownName           | String | 可选      | 接收方所在城镇，如 `San Jose`<br>适用于walletType=`exchange`                                                                                                                |
| > rcvrStreetName         | String | 可选      | 接收方所在街道地址，如 `Clementi Avenue 1`<br>适用于walletType=`exchange`                                                                                                     |

> 返回结果

```json
{
        "code": "0",
        "msg": "",
        "data": [{
                "wdId": "121212",
                "cTime": "1597026383085"
        }]
}
```
#### 返回参数

| 参数名 | 类型   | 描述          |
| :----- | :----- |:------------|
| wdId   | String | 提币申请 ID     |
| cTime  | String | 提币申请 ID生成时间 |
