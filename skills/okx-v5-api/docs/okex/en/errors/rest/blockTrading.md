---
site: okex
lang: en
name: Block Trading and Spread Orderbook
type: guide
---

### Block Trading and Spread Orderbook

Error Code from 70000

|	 Error Code 	|	 HTTP Status Code 	|	 Error Message                                                                                                                   	|
|	 :---------- 	|	 :---------------- 	|	 :------------------------------------------------------------------------------------------------------------------------------- 	|
|	70000	|	200	|	 RFQ does not exist.                                       	|
|	70001	|	200	|	 Quote does not exist.                                     	|
|	70002	|	200	|	 Block trade does not exist.                               	|
|	70003	|	200	|	 Public block trade does not exist.                        	|
|	70004	|	200	|	 Invalid instrument {instId}                                    	|
|	70005	|	200	|	 The number of legs in RFQ cannot exceed maximum value.    	|
|	70006	|	200	|	 Does not meet the minimum asset requirement.              	|
|	70007	|	200	|	 Underlying index {instFamily} does not exist under instType {instType}.   	|
|	70008	|	200	|	 Operation failed under MMP status. |
|	70009	|	200	|	 Data must have at least 1 valid element.                  	|
|	70010	|	200	|	 Timestamp parameters need to be in Unix timestamp format in milliseconds.                  	|
|	70011	|	200	|	 Duplicate setting for instType {instType}.                       	|
|	70012	|	200	|	 Duplicate setting for underlying/instId {instId} under the same instType {instType}.                       	|
|	70013	|	200	|	 endTs needs to be bigger than or equal to beginTs.                       	|
|	70014	|	200	|	 It's not allowed to have includeAll=True for all the instType.                       	|
|	70015	|	200	|	 In order to trade this product, you need to complete advanced verification                       	|
|	70016	|	200	|	 Please specify your instrument settings for at least one instType.                       	|
| 70060 | 200 | The {account} doesn’t exist or the position side is incorrect. “To” and “from” accounts must be under the same main account. |
| 70061 | 200 | To move position, please enter a position that’s opposite to your current side and is smaller than or equal to your current size. |
| 70062 | 200 | {account} has reached the maximum number of position transfers allowed per day. |
| 70064 | 200 | Position does not exist. |
| 70065 | 200 | Couldn’t move position. Execution price cannot be determined. |
| 70066 | 200 | Moving positions isn't supported in spot mode. Switch to any other account mode and try again. |
| 70067 | 200 | Moving positions isn't supported in margin trading. |
|	70100	|	200	|	 Duplicate instruments in legs array.                      	|
|	70101	|	200	|	 Duplicate clRfqId                                         	|
|	70102	|	200	|	 No counterparties specified                               	|
|	70103	|	200	|	 Invalid counterparty                                      	|
|	70105	|	200	|	 The total value of non all-SPOT RFQs should be greater than the min notional value {nonSpotMinNotional} 	|
|	70106	|	200	|	 The trading amount does not meet the min tradable amount requirement 	|
|	70107	|	200	|	 The number of counterparties cannot exceed maximum value. 	|
|	70108	|	200	|	 The total value of all-spot RFQs should be greater than the min notional value {spotMinNotional} 	|
|	70109	|	200	|	 Counterparties for selected instruments are currently unavailable. 	|
|	70200	|	200	|	 The RFQ with {rfqState} status cannot be canceled                	|
|	70203	|	200	|	 Cancellation failed as rfq count exceeds the limit {rfqLimit}.   	|
|	70207	|	200	|	 Cancellation failed as you do not have any active RFQs.   	|
|	70208	|	200	|	 Cancellation failed as service is unavailable now, please try again later.	|
|	70301	|	200	|	 Duplicate clQuoteId.                                      	|
|	70303	|	200	|	 The RFQ with {rfqState} status cannot be quoted.                 	|
|	70304	|	200	|	 Price should be an integer multiple of the tick size.     	|
|	70305	|	200	|	 Bid price cannot be higher than offer price               	|
|	70306	|	200	|	 The legs of quote do not match the legs of {rfqId}            	|
|	70307	|	200	|	 Size should be in integral multiples of the lot size.     	|
|	70308	|	200	|	 Quote to your own RFQ is not allowed.                     	|
|	70309	|	200	|	 Quote to the same RFQ with the same side is not allowed.  	|
|	70310	|	200	|	 Quoted price of instId {instId} cannot exceed your preset price limit.  	|
|	70400	|	200	|	 The Quote with {quoteState} status cannot be canceled              	|
|	70408	|	200	|	 Cancellation failed as quote count exceeds the limit {quoteLimit}. 	|
|	70409	|	200	|	 Cancellation failed as you do not have any active Quotes. 	|
|	70501	|	200	|	 RFQ {rfqId} is not quoted by {quoteId}                              	|
|	70502	|	200	|	 The legs do not match the legs of {rfqId}                     	|
|	70503	|	200	|	 Leg sizes specified are under the minimum block size required by Jupiter.        	|
|	70504	|	200	|	 Execution failed as the RFQ status is {rfqState}.                	|
|	70505	|	200	|	 Execution failed as the Quote status is {quoteState}.              	|
|	70506	|	200	|	 Leg sizes specified do not have the same ratios as the whole RFQ.              	|
|	70507	|	200	|	 Partial execution was attempted but allowPartialExecution of the RFQ is not enabled.              	|
|	70508	|	200	|	 No instrument settings available.              	|
|	70509	|	200	|	 Execution failed: counterparty error              	|
|   70510   |   200 |    For error details, refer to the acctAlloc field. |
|	70511	|	200	|	 Execution is being processed                              	|
| 70514 	|	200	|	  For each symbol, the total size of RFQ legs in all accounts should be equal to its combined amount in the group RFQ. |
| 70515 	|	200	|	  For each sub-account, the ratio of a leg’s size to the main account RFQ must be the same across all symbols. |
| 70516 	|	200	|	  You can only select up to {param0} sub-accounts for group RFQ. |
| 70517 	|	200	|	  {param0} doesn't exist or you don’t have permission to create group RFQ for it. |
| 70518 	|	200	|	  Make sure you didn’t select the same account more than once for group RFQ. |
|	75001	|	200	|	 Trade ID does not exist                              	|
|	75002	|	200	|	 {sprdId} : unable to place new orders or modify existing orders at the moment                              	|
|	75003	|	200	|	 Invalid price                              	|
|	56000	|	200	|	 Block trade does not exist.                               	|
|	56001	|	200	|	 The number of multi-legs cannot exceed {legLimit}.               	|
|	56002	|	200	|	 The number of multi-legs does not match with the verified one.	|
|	56003	|	200	|	 Duplicate clBlockTdId.                                   	|
|	56004	|	200	|	 Trade with yourself is not allowed.                       	|
|	56005	|	200	|	 clBlockTdId should be the same as the verified one.       	|
|	56006	|	200	|	 The role should be different from the verified one.       	|
|	56007	|	200	|	 Leg no.{legNo} does not match with the verified one.          	|
|	56008	|	200	|	 Duplicate instruments in legs array.                      	|
