---
site: okex
lang: zh
name: 跟单
type: guide
---

## 跟单 [order-book-trading-copy-trading]

带单 API 交易工作流程如下：<br><br>

**1. 申请成为带单交易员**<br>

* 申请流程可以参考 [如何申请成为交易员](https://www.okx.com/cn/help/11639154398221)；<br>
* 可通过[查看账户配置](/docs-v5/zh/#trading-account-rest-api-get-account-configuration)接口的`roleType` 或者 `spotRoleType` 是否为 1，判断当前账户是否为带单交易员。<br>

**2. 带单合约**<br>

* [获取带单产品](/docs-v5/zh/#order-book-trading-copy-trading-get-leading-instruments)接口，用于查看平台哪些合约支持带单，以及您开启了哪些合约的带单。对于您未开启带单的合约，依旧可以正常交易，只是不会触发跟单；<br>
* [交易员修改带单合约](/docs-v5/zh/#order-book-trading-copy-trading-post-amend-leading-instruments)接口，初始带单合约在申请带单交易员时进行设置，该接口用于修改您的带单合约。非带单合约修改为带单合约时，该次请求中所有的非带单合约合约不能有持仓或者挂单。<br>

**3. 开仓**

  * 需要通过下单接口和频道进行开仓，包括：[下单](/docs-v5/zh/#order-book-trading-trade-post-place-order)接口、[批量下单](/docs-v5/zh/#order-book-trading-trade-post-place-multiple-orders)接口、[下单频道](/docs-v5/zh/#order-book-trading-trade-ws-place-order)、[批量下单频道](/docs-v5/zh/#order-book-trading-trade-ws-place-multiple-orders)。<font color="red">现货带单时，`tdMode` 的值需要指定为`spot_isolated`</font>
  * 在买卖模式下，委托的方向必须与现有持仓和挂单保持一致，如果对应产品没有持仓和挂单，可根据自己的需求选择委托方向；
  * 开平仓模式下，可根据自己的需求选择开多或开空。

**4. 平仓**

  * 可以通过下单接口和频道进行平仓，支持自定义价格和数量，包括：[下单](/docs-v5/zh/#order-book-trading-trade-post-place-order)接口、[批量下单](/docs-v5/zh/#order-book-trading-trade-post-place-multiple-orders)接口、[下单频道](/docs-v5/zh/#order-book-trading-trade-ws-place-order)、[批量下单频道](/docs-v5/zh/#order-book-trading-trade-ws-place-multiple-orders)，也可以通过[市价仓位全平](/docs-v5/zh/#order-book-trading-trade-post-close-positions)接口或者[平仓带单](/docs-v5/zh/#order-book-trading-copy-trading-post-close-lead-position)接口进行平仓；
  * [市价仓位全平](/docs-v5/zh/#order-book-trading-trade-post-close-positions)接口，平掉当前产品下指定的仓位（如：开平模式下，全仓模式下的多仓或空仓），可能包含多个带单；
  * [平仓带单](/docs-v5/zh/#order-book-trading-copy-trading-post-close-lead-position)接口，一次仅平仓某一个带单仓位。带单ID（subPosId）为必填参数，需要通过[获取当前带单](/docs-v5/zh/#order-book-trading-copy-trading-get-existing-lead-positions)接口获取。

**5. 止盈止损**

  * 可以通过[带单仓位止盈止损](/docs-v5/zh/#order-book-trading-copy-trading-post-place-lead-stop-order)接口或者[策略委托下单](/docs-v5/zh/#order-book-trading-algo-trading-post-place-algo-order)接口设置止盈止损；
  * [ 带单仓位止盈止损](/docs-v5/zh/#order-book-trading-copy-trading-post-place-lead-stop-order)接口，一次仅为一个带单仓位设置。带单ID（subPosId）为必填参数，需要通过[获取当前带单](/docs-v5/zh/#order-book-trading-copy-trading-get-existing-lead-positions)接口获取。
  * [策略委托下单](/docs-v5/zh/#order-book-trading-algo-trading-post-place-algo-order)接口，为当前产品下指定的仓位（如：开平模式下，全仓模式下的多仓或空仓）设置，可能包含多个带单；
