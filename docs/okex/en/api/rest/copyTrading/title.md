---
site: okex
lang: en
name: Copy Trading
type: guide
---

## Copy Trading

Lead trading API Workflow as follows:<br><br>

**1. Apply to become a leading trader.**

* The procedure can refer to [How to become a lead trader](https://www.okx.com/help/11639154398221);<br>
* You can know whether you are a lead trader by checking whether `roleType` or `spotRoleType` from [Get account configuration](/docs-v5/en/#trading-account-rest-api-get-account-configuration) is 1.

**2. Leading instruments:**

* [GET / Leading instruments](/docs-v5/en/#order-book-trading-copy-trading-get-leading-instruments) can get instruments that are supported to have leading trades and the instruments that you enable leading trade. For instruments that are disenabled copy trading, you can still trade normally, but copy trading will not be triggered;<br>
* [Amend leading instruments](/docs-v5/en/#order-book-trading-copy-trading-amend-leading-instruments) can amend your leading instruments. You need to set initial leading instruments while applying to become a leading trader. All non-leading contracts can't have position or pending orders for the current request when setting non-leading contracts as leading contracts.<br>

**3. Open position:** 

* You can open the position by placing order endpoints and channels including [Place order](/docs-v5/en/#order-book-trading-trade-post-place-order) endpoint, [Place multiple orders](/docs-v5/en/#order-book-trading-trade-post-place-multiple-orders) endpoint, [Place order channel](/docs-v5/en/#order-book-trading-trade-ws-place-order), [Place multiple orders channel](/docs-v5/en/#order-book-trading-trade-ws-place-multiple-orders), <font color="red">`tdMode` should be `spot_isolated` for `SPOT` lead trading.</font> <br>
* For buy/sell mode, the orders must be in the same direction as your existing positions and open orders. You can select the direction you want if the instrument does not have position and pending orders.
* For long/short mode, you can open long or open short as you want.

**4. Close position**

* You can close the position with customized price or size by placing order endpoints and channels including [Place order](/docs-v5/en/#order-book-trading-trade-post-place-order) endpoint, [Place multiple orders](/docs-v5/en/#order-book-trading-trade-post-place-multiple-orders) endpoint, [Place order channel](/docs-v5/en/#order-book-trading-trade-ws-place-order), [Place multiple orders channel](/docs-v5/en/#order-book-trading-trade-ws-place-multiple-orders), or close the position by [Close positions](/docs-v5/en/#order-book-trading-trade-post-close-positions) / [Close lead position](/docs-v5/en/#order-book-trading-copy-trading-post-close-lead-position);<br>
* [Close positions](/docs-v5/en/#order-book-trading-trade-post-close-positions) can close certain position under the current instrument(e.g. the long or short position under long/shor mode ), which can contain multiple leading positions;<br>
* [Close lead position](/docs-v5/en/#order-book-trading-copy-trading-post-close-lead-position) can only close a leading position once a time. It is required to pass subPosId which can get from [Get existing leading positions](/docs-v5/en/#order-book-trading-copy-trading-get-existing-lead-positions).

**5. TP/SL**

* TP/SL can be set by [Place algo order](/docs-v5/en/#order-book-trading-trade-ws-mass-cancel-order) or [Place lead stop order](/docs-v5/en/#order-book-trading-copy-trading-post-place-lead-stop-order);<br>
* [Place algo order](/docs-v5/en/#order-book-trading-trade-ws-mass-cancel-order) can set TP/SL for certain position under the current instrument(e.g. the long or short position under long/shor mode ), which can contain multiple leading positions;<br>
* [Place lead stop order](/docs-v5/en/#order-book-trading-copy-trading-post-place-lead-stop-order) set set TP/SL for only a leading position once a time. It is required to pass subPosId which can get from [Get existing leading positions](/docs-v5/en/#order-book-trading-copy-trading-get-existing-lead-positions).
