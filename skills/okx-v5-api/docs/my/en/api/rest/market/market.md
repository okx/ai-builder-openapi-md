---
site: my
lang: en
name: Market Data
type: guide
---

## Market Data

The API endpoints of `Market Data` do not require authentication.

There are multiple services for market data, and each service has an independent cache. A random service will be requested for every request. So for two requests, it’s expected that the data obtained in the second request is earlier than the first request.

For event contracts, market data module will only return data of the YES side. Users can derive NO side data on their own.
