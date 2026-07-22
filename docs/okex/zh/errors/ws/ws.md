---
site: okex
lang: zh
name: WebSocket
type: guide
---

## WebSocket [error-code-websocket]

WebSocket 错误消息均为英文，中文错误消息仅供参考

### 公共 [error-code-websocket-public]

错误码从 60000 到 64002

#### 通用类

| 	 错误码 	   | 	 错误消息                               	                                                                                                                                                                                                     |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 	60004	   | 	 无效的 timestamp             	                                                                                                                                                                                                              |
| 	60005	   | 	 无效的 apiKey                   	                                                                                                                                                                                                           |
| 	60006	   | 	 请求时间戳过期                         	                                                                                                                                                                                                        |
| 	60007	   | 	 无效的签名                             	                                                                                                                                                                                                      |
| 	60008	   | 	 当前服务不支持订阅{0}频道，请检查WebSocket地址                                                                                                                                                                                                            |
| 	60009	   | 	 登录失败                               	                                                                                                                                                                                                     |
| 	60011	   | 	 用户需要登录                           	                                                                                                                                                                                                       |
| 	60012	   | 	 不合法的请求                           	                                                                                                                                                                                                       |
| 	60013	   | 	 无效的参数 args                        	                                                                                                                                                                                                      |
| 	60014	   | 	 用户请求频率过快，超过该接口允许的限额 	                                                                                                                                                                                                                    |
| 	60018	   | 	 错误的 URL 或者 {0} 不存在，请参考 API 文档使用正确的 URL，频道和参数                             	                                                                                                                                                                                                      |
| 	60019	   | 	 无效的op{0}                           	                                                                                                                                                                                                     |
| 	60023	   | 	 批量登录请求过于频繁                      	                                                                                                                                                                                                        |
| 	60024	   | 	 passphrase不正确                      	                                                                                                                                                                                                     |
| 	60026	   | 	 不支持APIKey和token同时登录                               	                                                                                                                                                                                      |
| 	60027	   | 	 参数{0}不可为空                                          	                                                                                                                                                                                     |
| 	60028	   | 	 当前服务不支持此功能，请检查WebSocket地址                                                                                                                                                                                                                |
| 60031     | WebSocket地址不支持多账户和重复登录                                                                                                                                                                                                                        |
| 60032     | API key 不存在                                                                                                                                                                                                                                |
| 60033     | {param0} 参数错误                                                                                                                                                                                                                                |
| 	63999	   | 	 由于内部错误，登录失败，请稍后重试。                           	                                                                                                                                                                                      |
| 	64000	   | 	 订阅参数 uly 已失效，请您尽快将 uly 替换为 instFamily，更多详情可参考: https://www.okx.com/cn/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	                                     |
| 	64001	   | 	 该频道到已经迁移到了 '/business' URL，请使用新的 URL。更多详情可参考：https://www.okx.com/cn/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	                                       |
| 	64002	   | 	 "/business" URL 不支持该频道. 请使用"/private" URL(对于私有频道), 或者"/public" URL(对于公有频道)，更多详情可参考：https://www.okx.com/cn/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	 |
| 	64003	        | 	 用户交易费等级不支持访问该频道                                                      	 |
| 64004 | 不允许为 {instId} 同时订阅 {channelName} 以及 books-l2-tbt。请先取消订阅 books-l2-tbt。 |
| 	64007	        | 	 由于 WebSocket 内部错误导致操作 {0} 失败，请稍后再试。                                                      	 |
| 	64008	        | 	 因服务升级，该连接即将关闭。请重新连接。                                                      	 |

#### 关闭帧

| 状态码  | 文案                    |
|------|-----------------------|
| 1009 | 用户订阅请求过大              |
| 4001 | 登录失败                  |
| 4002 | 参数不合法                 |
| 4003 | 登录账户多于100个            |
| 4004 | 空闲超时30秒               |
| 4005 | 写缓冲区满                 |
| 4006 | 异常场景关闭                |
| 4007 | API key已更新或删除，请重新连接  |
| 4008 | 总订阅频道数量超过最大限制  |
| 4009 | 该连接订阅频道数超限制  |
