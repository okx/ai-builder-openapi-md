---
site: okex
lang: zh
name: 概览
type: guide
---

# 概览 [overview]

欢迎查看 API 文档。我们提供完整的REST和WebSocket API以满足您的交易需求。<br>

<aside class="warning">
<b>【重要】欧易 API 协议：继续使用即视为接受</b><br><br>

我们建议您在使用 API 服务前完整阅读<a href="https://www.okx.com/help/okx-api-agreement">《欧易 API 协议》</a>，继续使用任何欧易 API 服务即视为您完全接受本协议。<br>

若您不同意本协议，您必须立即停止使用所有欧易 API 服务，包括：断开所有活跃的 API 集成；断开 Agent Trade Kit 连接；关闭通过 API 访问欧易的自动化系统。
</aside>

<aside class="warning">
<b>【重要】区域 API 域名要求</b><br><br>
根据您的注册地区，进行 API 调用和查阅文档时<b>必须</b>使用对应的域名。<br>
以下地区用户使用 <code>openapi.okx.com</code> 将无法正常访问。<br><br>
<b>美国及澳大利亚用户</b>（在 <a href="https://app.okx.com">app.okx.com</a> 完成注册）：请使用 <code>us.okx.com</code> 作为 API 域名。请参阅 <a href="https://app.okx.com/docs-v5/en/">美国及澳大利亚 API 文档</a>。<br>
 <b>欧盟用户</b>（在 <a href="https://my.okx.com">my.okx.com</a> 完成注册）：请使用 <code>eea.okx.com</code> 作为 API 域名。请参阅 <a href="https://my.okx.com/docs-v5/en/">欧盟 API 文档</a>。<br>
</aside>

## API学习资源与技术支持 [overview-api-resources-and-support]

### 教程 [overview-api-resources-and-support-tutorials]
- 学习使用 API 交易: [API 使用指南](/docs-v5/trick_zh/#instrument-configuration)
- 学习使用Python交易现货: [Python 现货交易教程](/help/how-can-i-do-spot-trading-with-the-jupyter-notebook)
- 学习使用Python交易衍生品: [Python 衍生品交易教程](/help/how-can-i-do-derivatives-trading-with-the-jupyter-notebook)

<br>

### Python包 [overview-api-resources-and-support-python-libraries]
- 使用Python SDK更简单地上手: [Python SDK](https://pypi.org/project/python-okx/)
- 轻松上手Python做市商代码 [Python 做市商代码示例](https://github.com/okxapi/okx-sample-market-maker)
<br>

### 客户服务 [overview-api-resources-and-support-customer-service]

- 如有问题,您可以通过欧易 App 扫码进群与技术专员联系。<br>
打开 App，点击左上角全功能图标，再点击右上角扫码图标，扫描右侧二维码即可。

<p><img src="images/APISupport.jpeg" alt="API 技术支持二维码" style="max-width: 300px; display: block; margin: 20px auto;" /></p>

## 创建我的APIKey [overview-v5-api-key-creation]

点击跳转至官网创建V5APIKey的页面 [创建我的APIKey](/account/my-api)<br>

### 生成APIKey [overview-v5-api-key-creation-generating-an-api-key]

在对任何请求进行签名之前，您必须通过交易网站创建一个APIKey。创建APIKey后，您将获得3个必须记住的信息：

- APIKey
- SecretKey
- Passphrase

APIKey和SecretKey将由平台随机生成和提供，Passphrase将由您提供以确保API访问的安全性。平台将存储Passphrase加密后的哈希值进行验证，但如果您忘记Passphrase，则无法恢复，请您通过交易网站重新生成新的APIKey。<br><br>

### APIKey 权限 [overview-v5-api-key-creation-api-key-permissions]
APIKey 有如下3种权限，一个 APIKey 可以有一个或多个权限。

- 读取 ：查询账单和历史记录等 读权限
- 提现 ：可以进行提币
- 交易 ：可以下单和撤单，转账，调整配置 等写权限

### APIKey 安全性 [overview-v5-api-key-creation-api-key-security]

<aside class="warning">
为了提高安全性，我们建议您将 APIKey 绑定 IP<br>
</aside>

- 每个APIKey最多可绑定20个IP地址，IP地址支持IPv4/IPv6和网段的格式。
<br>

<aside class="notice">
未绑定IP且拥有交易或提币权限的APIKey，将在闲置14天之后自动删除。(模拟盘的 API key 不会被删除)<br>
</aside>

- 用户调用了需要 APIKey 鉴权的接口，才会被视为 APIKey 被使用。
- 调用了不需要 APIKey 鉴权的接口，即使传入了 APIKey的信息，也不会被视为使用过。
- Websocket 只有在登陆的时候，才会被视为 APIKey 被使用过。在登陆后的连接中做任何操作（如 订阅/下单），也不会被认为 APIKey 被使用，这点需要注意。

用户可以在 [安全中心](/zh-hans/account/security) 中看到未绑定IP且拥有交易/提现权限的 APIKey 最近使用记录。

## REST 请求验证 [overview-rest-authentication]

### 发起请求 [overview-rest-authentication-making-requests]

所有REST私有请求头都必须包含以下内容：

- `OK-ACCESS-KEY`字符串类型的APIKey。

- `OK-ACCESS-SIGN`使用HMAC SHA256哈希函数获得哈希值，再使用Base-64编码（请参阅签名）。

- `OK-ACCESS-TIMESTAMP` 请求时间戳，ISO 8601 UTC格式，精确到毫秒，如：`2020-12-08T09:08:57.715Z`。服务器将拒绝与服务器时间相差超过30秒的请求（错误码50102）。请务必使用UTC时间——时区偏差是导致50102错误最常见的原因。建议在下单前通过 GET /api/v5/public/time 与服务器时间同步。

- `OK-ACCESS-PASSPHRASE`您在创建API密钥时指定的Passphrase。

所有请求都应该含有application/json类型内容，并且是有效的JSON。

### 签名 [overview-rest-authentication-signature]

> 生成签名

`OK-ACCESS-SIGN`的请求头是对`timestamp + method + requestPath + body`字符串（+表示字符串连接），以及SecretKey，使用HMAC SHA256方法加密，通过Base-64编码输出而得到的。

如：`sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp + 'GET' + '/api/v5/account/balance?ccy=BTC', SecretKey))`

其中，`timestamp`的值与`OK-ACCESS-TIMESTAMP`请求头相同，为ISO格式，如`2020-12-08T09:08:57.715Z`。

method是请求方法，字母全部大写：`GET/POST`。

requestPath是请求接口路径。如：`/api/v5/account/balance`

body是指请求主体的字符串，如果请求没有主体（通常为GET请求）则body可省略。如：`{"instId":"BTC-USDT","lever":"5","mgnMode":"isolated"}`

<aside class="notice">
GET请求参数是算作requestPath，不算body
</aside>

SecretKey为用户申请APIKey时所生成。如：`22582BD0CFF14C41EDBF1AB98506286D`

## WebSocket [overview-websocket]

### 概述 [overview-websocket-overview]

WebSocket是HTML5一种新的协议（Protocol）。它实现了用户端与服务器全双工通信， 使得数据可以快速地双向传播。通过一次简单的握手就可以建立用户端和服务器连接， 服务器根据业务规则可以主动推送信息给用户端。其优点如下：

- 用户端和服务器进行数据传输时，请求头信息比较小，大概2个字节。
- 用户端和服务器皆可以主动地发送数据给对方。
- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。

<aside class="notice"> 
强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。
</aside> 

### 连接 [overview-websocket-connect]

**连接限制**：3 次/秒 (基于IP)

当订阅公有频道时，使用公有服务的地址；当订阅私有频道时，使用私有服务的地址

**请求限制**：

每个连接 对于 `订阅`/`取消订阅`/`登录` 请求的总次数限制为 480 次/小时

<aside class="notice"> 
<p> 如果出现网络问题，系统会自动断开连接</p> 
<p> 如果连接成功后30s未订阅或订阅后30s内服务器未向用户推送数据，系统会自动断开连接</p> 
<p> 为了保持连接有效且稳定，建议您进行以下操作：</p> 
<p> 1. 每次接收到消息后，用户设置一个定时器，定时N秒，N 小于30。</p> 
<p> 2. 如果定时器被触发（N 秒内没有收到新消息），发送字符串 'ping'。</p> 
<p> 3. 期待一个文字字符串'pong'作为回应。如果在 N秒内未收到，请发出错误或重新连接。</p> 
</aside> 

### 连接限制 [overview-websocket-connection-count-limit]
子账户维度，订阅每个 WebSocket 频道的最大连接数为 30 个。每个 WebSocket 连接都由唯一的 connId 标识。

<br>

受此限制的 WebSocket 频道如下：

1. [订单频道] (/docs-v5/zh/#order-book-trading-trade-ws-order-channel)
2. [账户频道] (/docs-v5/zh/#trading-account-websocket-account-channel)
3. [持仓频道] (/docs-v5/zh/#trading-account-websocket-positions-channel)
4. [账户余额和持仓频道] (/docs-v5/zh/#trading-account-websocket-balance-and-position-channel)
5. [爆仓风险预警推送频道] (/docs-v5/zh/#trading-account-websocket-position-risk-warning)
6. [账户greeks频道] (/docs-v5/zh/#trading-account-websocket-account-greeks-channel)

若用户通过不同的请求参数在同一个 WebSocket 连接下订阅同一个频道，如使用 `{"channel": "orders", "instType": "ANY"}` 和 `{"channel": "orders", "instType": "SWAP"}`，只算为一次连接。若用户使用相同或不同的 WebSocket 连接订阅上述频道，如订单频道和账户频道。在该两个频道之间，计数不会累计，因为它们被视作不同的频道。简言之，系统计算每个频道对应的 WebSocket 连接数量。

<br>

新链接订阅频道时，平台将对该订阅返回`channel-conn-count`的消息同步链接数量。

> 链接数量更新

```json
{
    "event":"channel-conn-count",
    "channel":"orders",
    "connCount": "2",
    "connId":"abcd1234"
}

```

<br>

当超出限制时，一般最新订阅的链接会收到拒绝。用户会先收到平时的订阅成功信息然后收到`channel-conn-count-error`消息，代表平台终止了这个链接的订阅。在异常场景下平台会终止已订阅的现有链接。

> 链接数量限制报错

```json
{
    "event": "channel-conn-count-error",
    "channel": "orders",
    "connCount": "30",
    "connId":"a4d3ae55"
}

```

<br>

通过 WebSocket 进行的订单操作，例如下单、修改和取消订单，不会受到此改动影响。

### 登录 [overview-websocket-login]

> 请求示例

```shell
{
 "op": "login",
 "args":
  [
     {
       "apiKey": "******",
       "passphrase": "******",
       "timestamp": "1538054050",
       "sign": "7L+zFQ+CEgGu5rzCj4+BdV2/uUHGqddA9pI6ztsRRPs=" 
      }
   ]
}
```

#### 请求参数

| 参数      | 类型   | 是否必须 | 描述                                                |
| :-------- | :----- | :------- | :-------------------------------------------------- |
| op        | String | 是       | 操作，`login` |
| args      | Array of objectss  | 是       | 账户列表                              |
| > apiKey  | String | 是       | APIKey                                           |
| > passphrase     | String | 是       | APIKey 的密码                              |
| > timestamp      | String | 是       | 时间戳，Unix Epoch时间，单位是秒              |
| > sign           | String | 是       | 签名字符串                                  |

> 全部成功返回示例

```json
{
  "event": "login",
  "code": "0",
  "msg": "",
  "connId": "a4d3ae55"
}
```
> 全部失败返回示例

```json
{
  "event": "error",
  "code": "60009",
  "msg": "Login failed.",
  "connId": "a4d3ae55"
}
```

#### 返回参数

| 参数       | 类型   | 是否必须 | 描述                                                         |
| :--------- | :----- | :------- | :----------------------------------------------------------- |
| event      | String | 是       | 操作，`login` `error`                                         |
| code       | String | 否       | 错误码                                                         |
| msg        | String | 否       | 错误消息                                                       |
| connId   | String   | 是  | WebSocket连接ID |

**apiKey**:调用API的唯一标识。需要用户手动设置一个
**passphrase**:APIKey的密码
**timestamp**:Unix Epoch 时间戳，单位为秒，如 1704876947
**sign**:签名字符串，签名算法如下：

先将`timestamp` 、 `method` 、` requestPath` 进行字符串拼接，再使用HMAC SHA256方法将拼接后的字符串和SecretKey加密，然后进行Base64编码

**SecretKey:**用户申请APIKey时所生成的安全密钥，如：22582BD0CFF14C41EDBF1AB98506286D

**其中 timestamp 示例**:const timestamp = '' + Date.now() / 1,000

**其中 sign 示例**: sign=CryptoJS.enc.Base64.stringify(CryptoJS.HmacSHA256(timestamp +'GET'+ '/users/self/verify', secret))

**method**  总是 'GET'

**requestPath** 总是 '/users/self/verify'

<aside class="notice"> 
请求在时间戳之后30秒会失效，如果您的服务器时间和API服务器时间有偏差，推荐使用 REST API查询API服务器的时间，然后设置时间戳
</aside> 

### 订阅 [overview-websocket-subscribe]

**订阅说明**

> 请求格式说明

```shell
{
	"op": "subscribe",
	"args": ["<SubscriptionTopic>"]
}
```

WebSocket 频道分成两类： `公共频道` 和 `私有频道`

`公共频道`无需登录，包括行情频道，K线频道，交易数据频道，资金费率频道，限价范围频道，深度数据频道，标记价格频道等。

`私有频道`需登录，包括用户账户频道，用户交易频道，用户持仓频道等。

用户可以选择订阅一个或者多个频道，多个频道总长度不能超过 64 KB。

以下是一个请求参数的例子。每一个频道的请求参数的要求都不一样。请根据每一个频道的需求来订阅频道。

> 请求示例

```shell
{
    "op":"subscribe",
    "args":[
        {
            "channel":"tickers",
            "instId":"BTC-USDT"
        }
    ]
}

```

**请求参数**

| 参数           | 类型   | 是否必须 | 描述                                                                                               |
|:-------------| :----- | :------- |:-------------------------------------------------------------------------------------------------|
| op           | String | 是       | 操作，`subscribe`                                                                                   |
| args         | Array of objects  | 是       | 请求订阅的频道列表                                                                                        |
| > channel    | String | 是       | 频道名                                                                                              |
| > instType   | String | 否       | 产品类型<br>`SPOT`：币币  <br>`MARGIN`：币币杠杆<br>`SWAP`：永续<br>`FUTURES`：交割<br>`OPTION`：期权<br>`ANY`：全部     |
| > instFamily | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                        |
| > instId     | String | 否       | 产品ID                                                                                             |

> 返回示例	

```json
{
    "event": "subscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "accb8e21"
}
```

**返回参数**

| 参数       | 类型   | 是否必须 | 描述                                                                                         |
| :--------- | :----- | :------- |:-------------------------------------------------------------------------------------------|
| event      | String | 是       | 事件，`subscribe`    `error`                                                                  |
| arg      | Object | 否       | 订阅的频道                                                                                      |
| > channel  | String | 是       | 频道名                                                                                        |
| > instType | String | 否       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续<br>`FUTURES`：交割<br>`OPTION`：期权<br>`ANY`：全部 |
| > instFamily      | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                                      |
| > instId   | String | 否       | 产品ID                                                                                       |
| code       | String | 否       | 错误码                                                                                        |
| msg        | String | 否       | 错误消息                                                                                       |
| connId   | String   | 是  | WebSocket连接ID                                                                              |

### 取消订阅 [overview-websocket-unsubscribe]

可以取消一个或者多个频道

> 请求格式说明

```shell
{
	"op": "unsubscribe",
	"args": ["< SubscriptionTopic > "]
}
```

> 请求示例

```shell
{
  "op": "unsubscribe",
  "args": [
    {
      "channel": "tickers",
      "instId": "BTC-USDT"
    }
  ]
}
```

**请求参数**

| 参数       | 类型   | 是否必须 | 描述                                                         |
| :--------- | :----- | :------- | :----------------------------------------------------------- |
| op         | String | 是       | 操作，`unsubscribe`                                          |
| args     | Array of objects  | 是       | 取消订阅的频道列表                                           |
| > channel  | String | 是       | 频道名                                                       |
| > instType | String | 否       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`ANY`：全部 |
| > instFamily      | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                      |
| > instId   | String | 否       | 产品ID                                                       |

> 返回示例

```json
{
    "event": "unsubscribe",
    "arg": {
        "channel": "tickers",
        "instId": "BTC-USDT"
    },
    "connId": "d0b44253"
}
```

**返回参数**

| 参数           | 类型   | 是否必须 | 描述                                                         |
|:-------------| :----- | :------- | :----------------------------------------------------------- |
| event        | String | 是       | 事件，`unsubscribe`    `error`                               |
| arg          | Object | 否       | 取消订阅的频道                                               |
| > channel    | String | 是       | 频道名                                                       |
| > instType   | String | 否       | 产品类型<br>`SPOT`：币币<br>`MARGIN`：币币杠杆<br>`SWAP`：永续合约<br>`FUTURES`：交割合约<br>`OPTION`：期权<br>`ANY`：全部 |
| > instFamily | String | 否       | 交易品种<br>适用于`交割`/`永续`/`期权`                                                      |
| > instId     | String | 否       | 产品ID                                                       |
| code         | String | 否       | 错误码                                                       |
| msg          | String | 否       | 错误消息                                                     |
| connId       | String   | 是  | WebSocket连接ID |

### 通知 [overview-websocket-notification]

WebSocket有一种消息类型(event=`notice`)。 <br><br>

用户会在如下场景收到此类信息：

- Websocket服务升级断线

在推送服务升级前60秒会推送信息，告知用户WebSocket服务即将升级。用户可以重新建立新的连接避免由于断线造成的影响。

> 返回示例

```json
{
    "event": "notice",
    "code": "64008",
    "msg": "The connection will soon be closed for a service upgrade. Please reconnect.",
    "connId": "a4d3ae55"
}
```
<br><br>
目前支持WebSocket公共频道(/ws/v5/public)和私有频道(/ws/v5/private)。

## 账户模式 [overview-account-mode]
为了方便您的交易体验，请在开始交易前设置适当的账户模式。

交易账户交易系统提供四个账户模式，分别为`现货模式`、`合约模式`、`跨币种保证金模式`以及`组合保证金模式`。

账户模式的首次设置，需要在网页或手机app上进行。

## 实盘交易 [overview-production-trading-services]

实盘API交易地址如下：  

- REST：`https://openapi.okx.com`  
- WebSocket公共频道：`wss://ws.okx.com:8443/ws/v5/public`  
- WebSocket私有频道：`wss://ws.okx.com:8443/ws/v5/private`
- WebSocket业务频道：`wss://ws.okx.com:8443/ws/v5/business`

## 模拟盘交易 [overview-demo-trading-services]

目前可以进行 API 的模拟盘交易，部分功能不支持如`提币`、`充值`、`申购赎回`等。

  
模拟盘API交易地址如下：  

- REST：`https://openapi.okx.com` 
- WebSocket公共频道：`wss://wspap.okx.com:8443/ws/v5/public`  
- WebSocket私有频道：`wss://wspap.okx.com:8443/ws/v5/private`
- WebSocket业务频道：`wss://wspap.okx.com:8443/ws/v5/business`

模拟盘的账户与欧易的账户是互通的，如果您已经有欧易账户，可以直接登录。

模拟盘API交易需要在模拟盘上创建APIKey：

登录欧易账户—>交易—>模拟交易—>个人中心—>创建模拟盘APIKey—>开始模拟交易

<aside class="notice">
注意：模拟盘的请求的header里面需要添加 "x-simulated-trading: 1"。
</aside>

> 请求头示例

```wiki
Content-Type: application/json

OK-ACCESS-KEY: 37c541a1-****-****-****-10fe7a038418

OK-ACCESS-SIGN: leaVRETrtaoEQ3yI9qEtI1CZ82ikZ4xSG5Kj8gnl3uw=

OK-ACCESS-PASSPHRASE: 1****6

OK-ACCESS-TIMESTAMP: 2020-03-28T12:21:41.274Z

x-simulated-trading: 1
```

### 模拟盘交互式浏览器 [overview-demo-trading-services-demo-trading-explorer]

该功能接口用户需先登录，接口只会请求模拟环境

- Parameters 面板中点击`Try it out`按钮，编辑请求参数。

- 点击`Execute`按钮发送请求。Responses 面板中查看请求结果。

立即体验 [交互式浏览器](/demo-trading-explorer/v5/zh)

## 基本信息 [overview-general-info]

**交易所层面的下单规则如下：**

- 未成交订单（包括 post only，limit和处理中的taker单）的最大挂单数：4,000个
- 单个交易产品未成交订单的最大挂单数为500个，被计入到 500 笔挂单数量限制的**订单类型**包括：  
  - 限价委托 (Limit)
  - 市价委托 (Market)
  - 只挂单 (Post only)
  - 全部成交或立即取消 (FOK)
  - 立即成交并取消剩余 (IOC)
  - 市价委托立即成交并取消剩余 (optimal limit IOC)
  - 止盈止损 (TP/SL)
  - 以下类型的订单触发的限价和市价委托：
      - 止盈止损 (TP/SL)
      - 计划委托 (Trigger)
      - 移动止盈止损 (Trailing stop)
      - 套利下单 (Arbitrage)
      - 冰山策略 (Iceberg)
      - 时间加权策略 (TWAP)
      - 定投 (Recurring buy)

- 价差订单最大挂单数：所有价差订单挂单合计500个
- 策略委托订单最大挂单数：  

  - 止盈止损：100个 每个Instrument ID
  - 计划委托：500个  
  - 移动止盈止损：50个  
  - 冰山委托：100个  
  - 时间加权委托：20个  

- 网格策略最大个数：  

  - 现货网格：100个
  - 合约网格：100个  

<br>

**交易限制规则如下：**

- 当taker订单匹配的maker订单数量超过最大限制1000笔时，taker订单将被取消
  - 限价单仅成交与1000笔maker订单相对应的部分，并取消剩余；
  - 全部成交或立即取消（FOK）订单将直接被取消。

<br>

**返回数据规则如下：**

- 当返回数据中，有`code`，且没有`sCode`字段时，`code`和`msg`代表请求结果或者报错原因；

- 当返回中有`sCode`字段时，代表请求结果或者报错原因的是`sCode`和`sMsg`，而不是`code`和`msg`。

<br>

**`instFamily` 和 `uly` 参数说明：**

  - 以下说明以 `BTC` 合约为例，其他币种的合约同理。
  - `uly` 是指数，如："BTC-USD"，与盈亏结算和保证金币种 (`settleCcy`) 会存在一对多的关系。
  - `instFamily` 是交易品种，如：`BTC-USD_UM`，与盈亏结算和保证金币种 (`settleCcy`) 一一对应。
  - 以下表格详细展示了 `uly`, `instFamily`，`settleCcy` 和 `instId` 的对应关系。

| **合约类型** | **uly** | **instFamily** | **settleCcy** | **交割合约 instId** | **永续合约 instId** |
| --- | --- |--- |--- |--- | --- |
| USDT 本位合约 | BTC-USDT | BTC-USDT | USDT | BTC-USDT-250808 | BTC-USDT-SWAP |
| USDC 本位合约 | BTC-USDC | BTC-USDC | USDC | BTC-USDC-250808 | BTC-USDC-SWAP |
| USD 本位合约 | BTC-USD | **BTC-USD_UM** | **USDⓈ** | **BTC-USD_UM-250808** | **BTC-USD_UM-SWAP** |
| 币本位合约 | BTC-USD | **BTC-USD**  | **BTC** | **BTC-USD-250808** | **BTC-USD-SWAP** |

注意：<br>
1. USDⓈ 代表 USD 以及多种 USD 稳定币，如：USD, USDC, USDG。<br>
2. 盈亏结算和保证金币种指的[获取交易产品基础信息（私有）](/docs-v5/zh/#trading-account-rest-api-get-instruments)接口返回的 `settleCcy` 字段。

## 交易时效性 [overview-transaction-timeouts]

由于网络延时或者OKX服务器繁忙会导致订单无法及时处理。如果您对交易时效性有较高的要求，可以灵活设置请求有效截止时间`expTime`以达到你的要求。

（批量）下单，（批量）改单接口请求中如果包含`expTime`，如果服务器当前系统时间超过`expTime`，则该请求不会被服务器处理。

### REST API [overview-transaction-timeouts-rest-api]

请求头中设置如下参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| expTime | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085` |

目前支持如下接口：  

- [下单](/docs-v5/zh/#order-book-trading-trade-post-place-order)
- [批量下单](/docs-v5/zh/#order-book-trading-trade-post-place-multiple-orders)
- [修改订单](/docs-v5/zh/#order-book-trading-trade-post-amend-order)
- [批量修改订单](/docs-v5/zh/#order-book-trading-trade-post-amend-multiple-orders)
- 信号交易的 [POST / 下单](/docs-v5/zh/#order-book-trading-signal-bot-trading-post-place-sub-order)

> 请求示例

```wiki
curl -X 'POST' \
  'https://openapi.okx.com/api/v5/trade/order' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'OK-ACCESS-KEY: *****' \
  -H 'OK-ACCESS-SIGN: *****'' \
  -H 'OK-ACCESS-TIMESTAMP: *****'' \
  -H 'OK-ACCESS-PASSPHRASE: *****'' \
  -H 'expTime: 1597026383085' \   // 有效截止时间
  -d '{
  "instId": "BTC-USDT",
  "tdMode": "cash",
  "side": "buy",
  "ordType": "limit",
  "px": "1000",
  "sz": "0.01"
}'
```

### WebSocket [overview-transaction-timeouts-websocket]

请求中设置如下参数

| 参数名 | 类型   | 是否必须 | 描述                                               |
| :----- | :----- | :------- | :------------------------------------------------- |
| expTime | String | 否     | 请求有效截止时间。Unix时间戳的毫秒数格式，如 `1597026383085` |

目前支持如下接口：  

- [下单](/docs-v5/zh/#order-book-trading-trade-ws-place-order)
- [批量下单](/docs-v5/zh/#order-book-trading-trade-ws-place-multiple-orders)
- [修改订单](/docs-v5/zh/#order-book-trading-trade-ws-amend-order)
- [批量修改订单](/docs-v5/zh/#order-book-trading-trade-ws-amend-multiple-orders)

> 请求示例

```wiki
{
    "id": "1512",
    "op": "order",
    "expTime":"1597026383085",  // 有效截止时间
    "args": [{
        "side": "buy",
        "instId": "BTC-USDT",
        "tdMode": "isolated",
        "ordType": "market",
        "sz": "100"
    }]
}
```

## 限速 [overview-rate-limits]

我们的 REST 和 WebSocket API 使用限速来保护我们的 API 免受恶意使用，因此我们的交易平台可以可靠和公平地运行。<br>
当请求因限速而被我们的系统拒绝时，系统会返回错误代码 50011（用户请求频率过快，超过该接口允许的限额。请参考 API 文档并限制请求）。<br>
每个接口的限速都不同。 您可以从接口详细信息中找到每个接口的限制。 限速定义详述如下：

- WebSocket 登录和订阅限速基于连接。

- 公共未经身份验证的 REST 限速基于 IP 地址。

- 私有 REST 限速基于 User ID（子帐户具有单独的 User ID）。

- WebSocket 订单管理限速基于 User ID（子账户具有单独的 User ID）。

### 交易相关API [overview-rate-limits-trading-related-apis]

对于与交易相关的 API（下订单、取消订单和修改订单），以下条件适用：

- 限速在 REST 和 WebSocket 通道之间共享。

- 下单、修改订单、取消订单的限速相互独立。

- 限速在 Instrument ID 级别定义（期权除外）

- 期权的限速是根据 Instrument Family 级别定义的。 请参阅 [获取交易产品基础信息](/docs-v5/zh/#public-data-rest-api-get-instruments) 接口以查看交易品种信息。

- 批量订单接口和单订单接口的限速也是独立的，除了只有一个订单发送到批量订单接口时，该订单将被视为一个订单并采用单订单限速。

### 子账户限速 [overview-rate-limits-sub-account-rate-limit]

子账户维度，每2秒最多允许1000个订单相关请求。仅有新订单及修改订单请求会被计入此限制。此限制涵盖以下所列的所有接口。对于包含多个订单的批量请求，每个订单将被单独计数。如果请求频率超过限制，系统会返回50061错误码。产品ID维度的限速规则保持不变，现有的限速规则与新增的子账户维度限速将并行运行。若用户需要更高的速率限制，可以通过多个子账户进行交易。

* [POST / 下单](/docs-v5/zh/#order-book-trading-trade-post-place-order)
* [POST / 批量下单](/docs-v5/zh/#order-book-trading-trade-post-place-multiple-orders)
* [POST / 修改订单](/docs-v5/zh/#order-book-trading-trade-post-amend-order)
* [POST / 批量修改订单](/docs-v5/zh/#order-book-trading-trade-post-amend-multiple-orders)

* [WS / 下单](/docs-v5/zh/#order-book-trading-trade-ws-place-order)
* [WS / 批量下单](/docs-v5/zh/#order-book-trading-trade-ws-place-multiple-orders)
* [WS / 改单](/docs-v5/zh/#order-book-trading-trade-ws-amend-order)
* [WS / 批量改单](/docs-v5/zh/#order-book-trading-trade-ws-amend-multiple-orders)

### 基于成交比率的子账户限速 [overview-rate-limits-fill-ratio-based-sub-account-rate-limit]

仅适用于用户等级 >= VIP5的用户。
<br>
为了激励更高效的交易，交易所将为交易成交比率高的用户提供更高的子账户限速。

交易所将在每天 00:00 UTC，根据过去七天的交易数据计算两个比率。

1. 子账户成交比率：该比率为（子账户的USDT对应交易量）/（每个交易产品的新增和修改请求数 * 交易产品乘数之和）。请注意，在这种情况下，母账户自身也被视为一个“子账户”。
2. 母账户合计成交比率：该比率为（母账户层面的USDT对应交易量）/（所有子账户各个交易产品的新增和修改请求数 * 交易产品乘数之和）。

交易产品乘数允许我们微调每个交易产品对成交比率的影响权重。较小的交易产品乘数（<1）适用于小币对及合约，在交易这些币对、合约时，为达到相同交易量往往需要更多的订单。所有的交易产品都有默认乘数，部分交易产品有独立的乘数。详情请见下表。

| 业务线 | 覆盖规则 | 独立乘数 | 默认乘数 |
| --- | --- | --- | --- |
| 永续 | 产品ID | `1` <br>产品ID： <br>BTC-USDT-SWAP <br>BTC-USD-SWAP <br>ETH-USDT-SWAP <br>ETH-USD-SWAP | `0.2` |
| 交割 | 交易品种 | `0.3` <br>交易品种： <br>BTC-USDT <br>BTC-USD <br>ETH-USDT <br>ETH-USD | `0.1` |
| 币币 | 产品ID | `0.5` <br>产品ID： <br>BTC-USDT <br>ETH-USDT | `0.1` |
| 期权 | 交易品种 |  | `0.1` |

成交比率计算不包括大宗交易，价差交易，做市商保护（MMP），以及法币类型订单对应的订单数量；并且不包括大宗交易，价差交易对应的交易量。仅考虑 `sCode = 0` 的成功请求。<br>

每日 08:00 UTC，系统将根据UTC时间 00:00 的数据快照，选取子账户成交比率及母账户合计成交比率中的较大值决定子账户的未来限速。详情请见下表。对于独立经纪商，系统只会取子账户的成交比率。

|  | 成交比率[x<=比率<y) | 子账户每2秒限速(新订单及修改订单请求) |
| --- | --- | --- |
| Tier 1 | [0,1) | 1,000 |
| Tier 2 | [1,2) | 1,250 |
| Tier 3 | [2,3) | 1,500 |
| Tier 4 | [3,5) | 1,750 |
| Tier 5 | [5,10) | 2,000 |
| Tier 6 | [10,20) | 2,500 |
| Tier 7 | [20,50) | 3,000 |
| Tier 8 | >= 50 | 10,000 |

若成交比率和预期限速有所改善，则提升将于 08:00 (UTC) 立即生效。但若成交比率下降，需要降低未来限速，系统将给予一天的宽限期，降低后的速率限制将在 T+1 08:00 (UTC) 实施。在 T+1 时，若成交比率提高，则将立即授予更高的限速。若用户的交易手续费等级降级为 VIP4，其限速将降低为最低档位，并有一天的宽限期。

<br>

若子账户7日交易量低于1,000,000 USDT，则按照母账户的合计成交比率实施限速。

<br>

对于新创建的子账户，创建时将应用最低档位限速，在 T+1 08:00 (UTC)时，将开始应用上述限速规则。

<br>

大宗交易、价差交易、做市商保护（MMP）以及币币、币币杠杆订单不受子账户限速限制。

<br>

交易所提供 [GET / 获取账户限速] (/docs-v5/zh/#order-book-trading-trade-get-account-rate-limit) 接口以便用户查询成交比率以及限速数据，数据将于每天 08:00 (UTC) 更新。该接口将返回子账户成交比率，母账户合计成交比率，子账户当前限速以及 T+1 时的预期子账户限速（适用于限速降级）。

<br>

成交比率、限速计算样例如下。用户有三个账户，交易产品 BTC-USDT-SWAP 及 XRP-USDT 的乘数分别为1，0.1。

1. 账户 A（母账户）：
    1. BTC-USDT-SWAP 交易量为100 USDT，订单数量为10;
    2. XRP-USDT 交易量为20 USDT，订单数量为15;
    3. 子账户成交比率 = (100+20) / (10 * 1 + 15 * 0.1) = 10.4
2. 账户 B (子账户)：
    1. BTC-USDT-SWAP 交易量为200 USDT，订单数量为100;
    2. XRP-USDT 交易量为20 USDT，订单数量为30;
    3. 子账户成交比率 = (200+20) / (100 * 1 + 30 * 0.1) = 2.13
3. 账户 C (子账户)：
    1. BTC-USDT-SWAP 交易量为300 USDT，订单数量为100;
    2. XRP-USDT 交易量为20 USDT，订单数量为45;
    3. 子账户成交比率 = (300+20) / (100 * 1 + 45 * 0.1) = 3.06
4. 母账户合计成交比率 = (100+20+200+20+300+20) / (10 * 1 + 15 * 0.1 + 100 * 1 + 30 * 0.1 + 100 * 1 + 45 * 0.1) = 3.01
5. 账户限速
    1. 账户 A = max(10.4, 3.01) = 10.4 -> 2500订单请求/2秒
    2. 账户 B = max(2.13, 3.01) = 3.01 -> 1750订单请求/2秒
    3. 账户 C = max(3.06, 3.01) = 3.06 -> 1750订单请求/2秒

### 最佳实践 [overview-rate-limits-best-practices]
如果您需要的请求速率高于我们的限速，您可以设置不同的子账户来批量请求限速。 我们建议使用此方法来限制或间隔请求，以最大化每个帐户的限速并避免断开连接或拒绝请求。
