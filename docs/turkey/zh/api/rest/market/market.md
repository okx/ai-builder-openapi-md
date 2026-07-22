---
site: turkey
lang: zh
name: 行情数据
type: guide
---

## 行情数据 [order-book-trading-market-data]

`行情数据`功能模块下的API接口不需要身份验证。

行情数据存在多个服务且每个服务有独立的缓存，每次会随机请求到某一个服务，所以会存在两次请求，第二次获取到的数据早于第一次的情况。

针对事件合约，行情数据模块只返回YES侧的数据，用户可自行推导出NO侧数据。
