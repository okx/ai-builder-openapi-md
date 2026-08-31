---
site: okex
lang: en
name: Trading bot
type: guide
---

### Trading bot

Error Code from 55100 to 55999

| 	Error Code	 |	HTTP Status Code	|	Error Message	|
|--------------|	----------------	|	---------------------------------------------------------------------------	|
| 	55100	      | 	200	    | Take profit % should be within the range of {parameter1}-{parameter2}                                                   	                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 	55101	      | 	200	    | Stop loss % should be within the range of {parameter1}-{parameter2}                                                   	                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 	55102	      | 	200	    | Take profit % should be greater than the current bot’s PnL%                                                   	                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 	55103	      | 	200	    | Stop loss % should be less than the current bot’s PnL%                                                   	                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 	55104	      | 	200	    | Only futures grid supports take profit or stop loss based on profit percentage                                                  	                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 	55105	      | 	200	    | Increasing positions is not allowed under current status                                                  	                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 	55106	      | 	200	    | Increased amount should be within the range of {parameter1} - {parameter2}                                                  	                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 	55111	      |	200	|	 This signal name is in use, please try a new name               	|
| 	55112	      |	200	|	 This signal does not exist 	|
| 	55113	      |	200	|	 Create signal strategies with leverage greater than the maximum leverage of the instruments 	|
| 	55116	      |	200	|	 You can only place one chase order for each trading pair. 	|
