---
site: my
lang: en
name: WebSocket
type: guide
---

## WebSocket

### Public

Error Code from 60000 to 64002

#### General Class

| 	 Error Code 	 | 	 Error Message                                                               	                                                                                                                                                                                                                                           |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 	60004	        | 	 Invalid timestamp                                                	                                                                                                                                                                                                                                                      |
| 	60005	        | 	 Invalid apiKey                                                      	                                                                                                                                                                                                                                                   |
| 	60006	        | 	 Timestamp request expired                                                  	                                                                                                                                                                                                                                            |
| 	60007	        | 	 Invalid sign                                                               	                                                                                                                                                                                                                                            |
| 	60008	        | 	 The current WebSocket endpoint does not support subscribing to {0} channels. Please check the WebSocket URL                                                                                                                                                                                                             |
| 	60009	        | 	 Login failure                                                               	                                                                                                                                                                                                                                           |
| 	60011	        | 	 Please log in                                                              	                                                                                                                                                                                                                                            |
| 	60012	        | 	 Invalid request                                                            	                                                                                                                                                                                                                                            |
| 	60013	        | 	 Invalid args                                                             	                                                                                                                                                                                                                                              |
| 	60014	        | 	 Requests too frequent                                                      	                                                                                                                                                                                                                                            |
| 	60018	        | 	 Wrong URL or {0} doesn't exist. Please use the correct URL, channel and parameters referring to API document.                                     	                                                                                                                                                                                                                                                           |
| 	60019	        | 	 Invalid op: {op}                                                             	                                                                                                                                                                                                                                          |
| 	60023	        | 	 Bulk login requests too frequent                                               	                                                                                                                                                                                                                                        |
| 	60024	        | 	 Wrong passphrase                                                            	                                                                                                                                                                                                                                           |
| 	60026	        | 	 Batch login by APIKey and token simultaneously is not supported.            	                                                                                                                                                                                                                                           |
| 	60027	        | 	 Parameter {0} can not be empty.                                             	                                                                                                                                                                                                                                           |
| 	60028	        | 	 The current operation is not supported by this URL. Please use the correct WebSocket URL for the operation.     	                                                                                                                                                                                                                          |
| 60031          | The WebSocket endpoint does not allow multiple or repeated logins.                                                                                                                                                                                                                                                     |
| 60032          | API key doesn't exist.                                                                                                                                                                                                                                                     |
| 60033          | Parameter {param0} error.                                                                                                                                                                                                                                                     |
| 63999          | Login failed due to internal error. Please try again later.                                                                                                                                                                                                                                                     |
| 	64000	        | 	 Subscription parameter uly is unavailable anymore, please replace uly with instFamily. More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	                                                    |
| 	64001	        | 	 This channel has been migrated to the '/business' URL. Please subscribe using the new URL. More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	                                                |
| 	64002	        | 	 This channel is not supported by "/business" URL. Please use "/private" URL(for private channels), or "/public" URL(for public channels). More details can refer to: https://www.okx.com/help-center/changes-to-v5-api-websocket-subscription-parameter-and-url.                                                      	 |
| 	64003	        | 	 Your trading fee tier doesn't meet the requirement to access this channel                                                      	 |
| 64004 | Subscribe to both {channelName} and books-l2-tbt for {instId} is not allowed. Unsubscribe books-l2-tbt first. |
| 	64007	        | 	 Operation {0} failed due to WebSocket internal error. Please try again later.                                                      	 |
| 	64008	        | 	 The connection will soon be closed for a service upgrade. Please reconnect.                                                      	 |

#### Close Frame

| 	Status Code 	 | 	Reason Text 	|
|----------------| 	----------- 	|
| 1009           | Request message exceeds the maximum frame length |
| 4001           | Login Failed |
| 4002           | Invalid Request |
| 4003           | APIKey subscription amount exceeds the limit 100 |
| 4004           | No data received in 30s |
| 4005           | Buffer is full, cannot write data |
| 4006           | Abnormal disconnection |
| 4007           | API key has been updated or deleted. Please reconnect. |
| 4008           | The number of subscribed channels exceeds the maximum limit. |
| 4009           | The number of subscription channels for this connection exceeds the limit |
| 4013           | Compliance status has been updated. Please reconnect. |
