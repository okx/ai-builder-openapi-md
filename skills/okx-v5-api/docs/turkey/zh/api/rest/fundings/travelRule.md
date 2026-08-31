---
site: turkey
lang: zh
name: 获取旅行规则锁定的充值记录
method: GET
path: /api/v5/asset/travel-rule/get-locked
module: asset
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
multi_endpoint: true
---

## Endpoint sections

This generated file documents multiple REST endpoints. For a single-endpoint lookup, use this table or `rg -n -F "<search key>" <this-file>` to locate the target section, then read only that section instead of loading the whole file.

| Method | Path | Search key | Section |
| --- | --- | --- | --- |
| GET | `/api/v5/asset/travel-rule/get-locked` | `GET /api/v5/asset/travel-rule/get-locked` | 获取旅行规则锁定的充值记录 |
| POST | `/api/v5/asset/travel-rule/unlock` | `POST /api/v5/asset/travel-rule/unlock` | 解锁旅行规则充值 |

### 获取旅行规则锁定的充值记录 [funding-account-rest-api-get-locked-travel-rule-deposits]

获取当前被旅行规则锁定的充值列表。如果用户没有被锁定的充值，则返回空数组。锁定充值的最大数量限制为 50 条。

#### 限速：3次/s
#### 限速规则：User ID + IP

#### HTTP 请求

`GET /api/v5/asset/travel-rule/get-locked`

> 请求示例

```shell
GET /api/v5/asset/travel-rule/get-locked
```

```python
import okx.Funding as Funding

apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

result = fundingAPI.get_travel_rule_locked_deposits()
print(result)
```

#### 请求参数

无。用户身份通过 API Key 认证解析。

> 返回示例

```json
{
  "code": "0",
  "data": [
    {
      "depId": "98765",
      "txId": "0xabc123def456789...",
      "addr": "0xdef456abc789012...",
      "ccy": "USDT",
      "chain": "ERC20",
      "amt": "100.00000000",
      "ts": "1714564800000"
    }
  ],
  "msg": ""
}
```

#### 返回参数

| 参数  | 类型   | 描述                                                              |
| :---- | :----- | :---------------------------------------------------------------- |
| depId | String | 充值 ID                                                           |
| txId  | String | 链上交易哈希                                                      |
| addr  | String | 充值地址                                                          |
| ccy   | String | 币种，如 `USDT`。如果币种查询失败则返回空字符串                   |
| chain | String | 网络/链名称，如 `ERC20`。如果链查询失败则返回空字符串             |
| amt   | String | 充值数量                                                          |
| ts    | String | 数据生成时间，Unix 时间戳的毫秒数格式，如 `1714564800000`         |

---

### 解锁旅行规则充值 [funding-account-rest-api-unlock-travel-rule-deposit]

提交发起人身份信息以解锁被旅行规则锁定的充值。如果充值已经解锁，则直接返回成功，不重复处理。

#### 限速：3次/s
#### 限速规则：User ID + IP

#### HTTP 请求

`POST /api/v5/asset/travel-rule/unlock`

> 请求示例

```shell
POST /api/v5/asset/travel-rule/unlock
body
{
  "depId": "12345",
  "personType": "2",
  "trReason": "2",
  "walletType": "exchange",
  "exchName": "okx",
  "origFirstName": "John",
  "origLastName": "Doe",
  "origCountry": "TR",
  "origStreetName": "123 Main St",
  "origTownName": "Istanbul",
  "origCountrySubDivision": "Istanbul",
  "origPostCode": "34000"
}
```

```python
import okx.Funding as Funding

apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

result = fundingAPI.unlock_travel_rule_deposit(
    depId="12345",
    personType="2",
    trReason="2",
    walletType="exchange",
    exchName="okx",
    origFirstName="John",
    origLastName="Doe",
    origCountry="TR",
    origStreetName="123 Main St",
    origTownName="Istanbul",
    origCountrySubDivision="Istanbul",
    origPostCode="34000"
)
print(result)
```

#### 请求参数

| 参数                   | 类型   | 是否必须 | 描述                                                                                                                    |
| :--------------------- | :----- | :------- | :---------------------------------------------------------------------------------------------------------------------- |
| depId                  | String | 是       | 充值 ID，标识需要解锁的被冻结充值记录                                                                                   |
| personType             | String | 是       | 发起人类型<br>`1`：本人（存款人即账户持有人）<br>`2`：第三方                                                            |
| trReason               | String | 是       | 转账原因<br>`1`：私人钱包存储<br>`2`：个人投资<br>`3`：交易所转账<br>`4`：家庭支持                                     |
| walletType             | String | 是       | 来源钱包类型<br>`exchange`：交易所钱包<br>`private`：私人钱包                                                           |
| exchName               | String | 条件必填 | 来源交易所名称，如 `okx`。当 `walletType` = `exchange` 时必填                                                           |
| origFirstName          | String | 条件必填 | 发起人名字。未提供 `origCompanyName` 时必填                                                                             |
| origLastName           | String | 条件必填 | 发起人姓氏。未提供 `origCompanyName` 时必填                                                                             |
| origCompanyName        | String | 条件必填 | 发起人公司名称。未提供 `origFirstName` 和 `origLastName` 时必填                                                         |
| origCountry            | String | 是       | 发起人居住国家。ISO 3166-1 alpha-2 代码，如 `TR`                                                                        |
| origStreetName         | String | 是       | 发起人街道地址，如 `123 Main St`                                                                                        |
| origTownName           | String | 是       | 发起人所在城市/城镇                                                                                                     |
| origCountrySubDivision | String | 是       | 发起人所在州/省，如 `Istanbul`                                                                                          |
| origPostCode           | String | 条件必填 | 发起人邮政编码                                                                                                          |

> 返回示例

```json
{
  "code": "0",
  "data": [{"result": true}],
  "msg": ""
}
```

#### 返回参数

| 参数   | 类型    | 描述                                                                                     |
| :----- | :------ | :--------------------------------------------------------------------------------------- |
| result | Boolean | `true`：解锁成功 — 充值已释放或已提前解锁<br>空 `data` 数组：解锁失败 — 充值仍处于锁定状态 |
