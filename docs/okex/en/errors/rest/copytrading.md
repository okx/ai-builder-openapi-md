---
site: okex
lang: en
name: Copy trading
type: guide
---

### Copy trading

Error Code from 59200 to 59300

| 	 Error Code 	 |	 HTTP Status Code 	| 	 Error Message                                                                                                                                                                                     |
|----------------|	 ---------------- 	|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 	59128	        | 	200	                | As a lead trader, you can't lead trades in {instrument} with leverage higher than {num}×                                                   	                |
| 	59129	        | 	200	                | The first crypto you use to repay must be {param0}.                                                   	                |
| 	59130	        | 	200	                | If an asset’s balance is < 1 USD, it can only repay borrowings of the same crypto.                                                   	                |
| 	59206	        | 	200	                | The lead trader doesn't have any more vacancies for copy traders                                                  	                                                             |
| 	59216	        | 	200	                | The position doesn't exist. Please try again                                                  	                                                             |
| 	59218	        | 	200	                | Closing all positions at market price...                                                  	                                                                 |
| 	59256	        | 	200	                | To switch to One-way mode, lower the number of traders you copy to 1                                                  	                                                                |
| 59247          | 200                  | High leverage causes current position to exceed the maximum position size limit under this leverage. Adjust the leverage. |
| 	59260	        | 	200	                | You are not a spot lead trader yet. Complete the application on our website or app first.                                                   	                     |
| 	59262	        | 	200	                | You aren't a contract lead trader yet. Complete the application first.                                                   	                                        |
| 	59641	        | 	200	                | Can't switch account mode as you have fixed loan borrowings.                                                   	               |
| 	59642	        | 	200	                | Lead and copy traders can only use spot or futures modes                                                   	               |
| 	59643	        | 	200	                | Couldn’t switch account modes as you’re currently copying spot trades                                                   	      |
| 	59245	        | 	200	                | As a lead trader, number of {param0} contract per order must be no greater than {param1}                                                   	      |
| 	59263	        | 	200	    | Only traders on the allowlist can use copy trading. ND brokers can reach out to BD for help.                                                   	 |
| 	59264	        | 	200	    | Spot copy trading isn't supported                                                   	  |
| 	59267	        | 	200	    | Cancellation failed as you aren't copying this trader                                                   	                      |
| 	59268	        | 	200	    | You can't copy trades with instId that hasn't been selected by the lead trader                                                   	                                        |
| 	59269	        | 	200	    | This contract lead trader doesn't exist                                                   	      |
| 	59270	        | 	200	    | Maximum total amount (copyTotalAmt) can't be lower than amount per order (copyAmt) when using fixed amount                                                   	      |
| 	59273	        | 	200	    | You aren't a contract copy trader yet. Start by coping a contract trader.                                                   	      |
| 	59274	        | 	200	    | Copying your own trade isn't allowed                                                   	      |
| 	59275	        | 	200	    | You can't copy trade as you're applying to become a lead trader                                                   	      |
| 	59276	        | 	200	    | You can't copy this lead trader as they've applied to stop leading trades                                                   	      |
| 	59277	        | 	200	    | You can't copy this lead trader as they don't have any copy trader vacancies                                                   	      |
| 	59278	        | 	200	    | Your request to stop copy trading is being processed. Try again later.                                                   	      |
| 	59279	        | 	200	    | You've already copied this trader                                                   	       |
| 	59280	        | 	200	    | You can't modify copy trade settings as you aren't copying this trader                                                   	      |
| 	59282	        | 	200	    | Only ND sub-accounts under ND brokers whose main accounts are on the allowlist support this endpoint. Reach out to BD for help.                                                   	                                                                                                                                                                                            |
| 	59283	        | 	200	    | Your account isn't currently using futures mode                                                   	      |
| 	59284	        | 	200	    | You've reached the monthly limit of {param0} ratio edits                                                   	                                                                                                                                                                                                                                                                   |
| 	59286	        | 	200	    | You can't become a futures lead trader when using spot mode                                                   	                                                                                                                                                                                                                                                        |
| 	59287	        | 	200	    | Profit sharing ratio should be between {param0} and {param1}                                                   	                                                                                                                                                                                                                                                               |
| 	59288	        | 	200	    | You're leading trades but your account is in portfolio margin mode. Switch to futures mode or multiple-currency margin mode and try again.	                                                                                                                                                                                                                                  |
| 	59130	        | 	200	    | The highest take profit level is {num}%. Enter a smaller number and try again.                                                   	      |
| 	59258	        | 	200	    | Action not supported for lead traders                                                   	       |
| 	59259	        | 	200	    | Enter a multiplier value that's within the valid range                                                   	      |
| 	59285	        | 	200	    | You haven't led or copied any trades yet                                                   	      |
| 	59292	        | 	200	    | This lead trader only supports smart sync mode.                                                   	      |
