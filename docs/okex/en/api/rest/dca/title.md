---
site: okex
lang: en
name: DCA Trading
type: guide
---

## DCA Trading

A Martingale bot is a trading strategy that automatically adds to positions in batches as the market falls, thereby lowering the average holding cost. Users set the initial order amount, maximum number of safety orders, the price drop percentage that triggers each safety order, and the take-profit target. The bot will automatically place a buy order each time the price reaches a safety order condition, and close the position for profit once the price rebounds to the take-profit target.<br>
The API endpoints of `DCA Trading` require authentication.
