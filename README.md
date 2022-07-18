# DownBot
The bot buys tokens on strong falls, and averages according to a special algorithm.

Latest version link
( https://github.com/Bigbidbot/DownBot/releases )

## Bot setup
Works in console WINDOWS. There is no GUI. But in the next versions will be.
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++

YOUTUBE INSTRUCTION (https://www.youtube.com/channel/UCYm2p2WiP0xlpcDMygBRI8A/videos)

Telegram group (https://t.me/bigbidbotmarketinggroup) 
+++++++++++++++++++++++++++++++++++++++++++++++++++++++++


The bot works according to the following principle:

The bot will buy tokens even if the price drops sharply. He buys in an amount more than the previous one to raise in a linear order, i.e. if "BOT_STARTING_AMOUNT" = 50, then he buys for the first time at 50, and then 100, 150, 200, 250 until the price averages to the market. Further, if the price has risen to the balance (purchase) price, then he sells all additional purchases, except for the first one, and waits for a profit. If the price goes down again, then he starts averaging it again.

If the price immediately went up, then he tries to sell the tokens at the maximum price using (or not) the trailing stop method.
![DOwnBotSCREEN](https://user-images.githubusercontent.com/97591389/178146194-7bc3b2a6-8982-4007-be15-4be8634dc2f1.png)

More:

- The bot monitors the current price of the token and constantly calculates the average market price (AVG_PRICE) for the period specified in the "BOT_TIME_CALCULATION_AVG_PRICE_SECONDS" parameter. He only watches and does not buy anything. Tokan prices that are older than this period are not taken into account in the calculation of average prices. 
- If the current price of the token has become lower than the average market price (AVG_PRICE) by the percentage "BOT_DOWN_PRICE_PERSENT_FIRST", then the bot makes the first purchase for the amount "BOT_STARTING_AMOUNT" (Attention! If you have "MULTIBUY_REPURCHASES_FIRST" = "true" and "STOPLOSSHALFTRIGGER_START" more than 1, then the bot buys not 1 amount, but immediately for an amount equal to this parameter.For example, "STOPLOSSHALFTRIGGER_START" = "3" means 50+100+150 =300 dollars).
- If the price went up and reached "BOT_TAKE_PROFIT" as a percentage. Then the bot sells all the tokens on the balance. (Warning! If you use "BOT_TRAILING_STOP_ACTIVE"="true", then he tries to sell at the maximum price until he falls below the maximum (ATH) by the percentage "BOT_TRAILING_STOP_PERSENT")
- After each purchase/sale, the bot records its number in the "BOT_MAX_REPURCHASES_REALTIME" parameter. Purchases are carried out on an increase, i.e. 20, 40, 80, 100 ("BOT_STARTING_AMOUNT" * "BOT_MAX_REPURCHASES_REALTIME"). ATTENTION! You cannot change these parameters when the bot has bought something. the balance (purchase) price will be erroneous.
The bot will make purchases until it averages the price or until it reaches the "BOT_MAX_REPURCHASES" quantity. These are emergency purchases, and as soon as possible (see below when) the bot returns this money.
- When the price starts to rise and reaches the balance (purchase) AVG_BUY_PRICE plus the "STOPLOSSHALFPERCENT" percentage, the bot starts selling all emergency rebuys (up to "STOPLOSSHALFTRIGGER_START").
- If the price continues to fall then you can enable "STOPLOSSFULLTRIGGER" and then the bot will sell at a loss. We do not use this setting. It is better to give the bot more money to average. Usually the ratio is 1 to 5 (basic trading amount 300 Dollars then deposit 1500 Dollars)

In addition, algorithms can be imposed on the strategy:

 **trailingStop**: "BOT_TRAILING_STOP_ACTIVE" ="true". Those. We wait for the lowest price (ATL) and when it goes up by "BOT_TRAILING_STOP_PERSENT" we buy, and we wait for the highest price (ATH) and when it goes down by "BOT_TRAILING_STOP_PERSENT" we sell. ATH works regardless of the "BOT_TRAILING_STOP_ACTIVE_DOWN_REPURCHASES" parameter i.e. it always tries to sell high if TrailingStop is enabled. But it starts making purchases after "BOT_TRAILING_STOP_ACTIVE".


 **trend lines** (THE TESTING IS NOT AVAILABLE): If "TRENDLINESTATUS" is enabled, then the bot looks to see if the price was below "TRENDLINE1" or "TRENDLINE2". If TRENDLINE1_TRIGGER" is set to false, it means that this mark has not yet been passed and he will make a purchase (not paying attention to either the trailing stop or the percentage of the fall). He will not make a purchase only if your book (purchase) price is below this line. That is This is not profitable even if the market price of the token goes up from the bottom, i.e. grows.
 
 **LASTREBUY** (THE TESTING IS NOT AVAILABLE):
 **PRICE_FALL_BUY_ALGORITM** (THE TESTING IS NOT AVAILABLE):
 
 ## settings file

    "WALLETADDRESS": "Replace with your wallet address" - gives a test address to try.
    "PRIVATEKEY": "aes:nP+......Y49Gw==" "Replace with your wallet private key (not a phrase)" - gives a test address to probe. After the first launch, it will be encrypted with the password you choose.
    "USECUSTOMNODE": "true" - you are using a dedicated or shared server. Public free sometimes freezes. We automatically change them so that they do not block or freeze.
    "CUSTOMNODE": "http://192.0......" -- enter your paid server address
    "EXCHANGE": "pancakeswap" - Exchanger - only this one works so far.
    "EXCHANGEVERSION": "2" - exchanger version
    "PREAPPROVE": "true" - before starting, check if the bot is allowed to trade coins. If not, then a permission transaction will be performed. Should be left true.
    "ENCRYPTPRIVATEKEYS": "true" encrypt your private key. We highly recommend that no one can copy it if they get access to the settings files.
	"MY_CHAT_ID": "562111111" your "id" for telegram notifications about sales/purchases and other information from the bot. Go to @myidbot and type "/getid" to get your number. Go to @bigbidbot_information_bot   and press button "START"
	"SECRET_KEY_BOT": "one1111111111" -- your license key when purchasing the bot. This is a test one. Works with 1 token and a 5 second delay between price checks.
	"REAL_BUY_SELL_MODE": "true" --enable real selling. If you just want to see when the bot will buy and sell, set it to false.

 ## token file

        "STOPLOSSPRICEINBASE": - DO NOT TOUCH. the bot itself configures this parameter.
        "ENABLED": - enable/disable the operation of this token. Disabled tokens in the license will not be taken into account. ATTENTION! When you turn on the token, the bot must be completely restarted in order to correctly calculate the average price.
        "MAX_FAILED_TRANSACTIONS_IN_A_ROW": - how many bot can allow erroneous transactions for ALL TOKENS. For example, you don't have enough ether for a commission. Must be at least 0.1 BNB
        "LIQUIDITYINNATIVETOKEN": - what is the liquidity of the token - if the majority of the token is in bnb, set true
        "GASPRIORITY_FOR_ETH_ONLY": - ethereum settings.
        "SYMBOL": -- token name for display convenience
        "ADDRESS": 0x...... - token address
        "USECUSTOMBASEPAIR": "true" - if you want to trade and price in BUSD USDT then set it to true and configure the following 2 parameters.
        "BASESYMBOL": "BUSD" --name of the token for which you want to buy
        "BASEADDRESS": "0xe9e7cea3dedca5984780bafc599bd69add087d56" - address of the token for which you want to buy
        "BUYAMOUNTINBASE": "1" --DO NOT TOUCH. the bot itself configures this parameter.
        "BUYPRICEINBASE": "0.0000000001" - DO NOT TOUCH. the bot itself configures this parameter.
        "SELLAMOUNTINTOKENS": "all" -- DO NOT TOUCH. the bot itself configures this parameter.
        "SELLPRICEINBASE": "10000" --DO NOT TOUCH. the bot itself configures this parameter.
        "MAXTOKENS": "1000000000000" --DO NOT TOUCH. the bot itself configures this parameter.
        "MOONBAG": "0" --DO NOT TOUCH. the bot itself configures this parameter.
        "SLIPPAGE": "2" - slippage when selling or buying
        "HASFEES": "false" --DO NOT TOUCH IF YOU DON'T UNDERSTAND WHAT IT IS. If you are trading with coins with a commission, you must enable this parameter. ATTENTION! You can lose your funds if you disable this feature, and the token is with a commission. Test before you start.
        "GAS": "5" - transaction gas.
        "BOOSTPERCENT": "0" --DO NOT TOUCH IF YOU DON'T UNDERSTAND WHAT IT IS. Percentage of gas price in ethereum.
        "GASLIMIT": "2500000" -- transaction gas limit.
        "BOT_MAX_REPURCHASES": - the maximum number of purchases the bot can make.
        "BOT_MAX_REPURCHASES_REALTIME": - number of purchases made so far.
        "BOT_STARTING_AMOUNT": - amount of the first purchase (step) in dollars
        "MULTIBUY_REPURCHASES_FIRST": - if enabled, then the first purchase is made for the amount of "STOPLOSSHALFTRIGGER_START" * "BOT_STARTING_AMOUNT"
        "BOT_TAKE_PROFIT": - percentage of profit at which to sell.
        "BOT_DOWN_PRICE_PERSENT_FIRST": - percentage down from the average market price (AVG) for the first purchase
        "BOT_DOWN_PRICE_PERSENT_NEXT": - Percentage of fall from AVG-buy (average purchase, book price) for subsequent purchases
        "BOT_TIME_CALCULATION_AVG_PRICE_SECONDS": - time during which the average market price is calculated. Those. token prices that are older than this time are not taken into account in the calculation. Attention, if you are working in test mode, divide by 5 times, because delay for 5 seconds.
        "STOPLOSSHALFTRIGGER": - enable emergency purchase SALES. If set to false, then the bot only buys more and waits for the price to increase to the full "BOT_TAKE_PROFIT".
        "STOPLOSSHALFPERCENT": - the percentage by which the balance (purchase) price must be higher to start selling emergency purchases.
        "STOPLOSSHALFTRIGGER_START": - Until which purchase to sell emergency purchases. When the algorithm stops working, partial buys and sells.
        "STOPLOSSFULLTRIGGER": - sell everything at a loss, if the price has not gone down after all the averaging. We do not recommend turning it on if it is clearly not necessary in your strategy.
        "BOT_TRAILING_STOP_PERSENT": - the percentage by which the token can "jump" from ATH (High all the time) or ATL (Low all the time) so as not to sell / buy
        "BOT_TRAILING_STOP_ACTIVE_DOWN_REPURCHASES": - on which repurchase the trailing stop is activated if it is enabled. With growth, he always tries to sell at a higher price even at the first one.
        "BOT_TRAILING_STOP_ACTIVE": - enabling the algorithm.
        
        The following settings are under testing - they are locked, but should be in the file.
        
	"TRENDLINESTATUS": - enabling the algorithm
        "TRENDLINE1": 0.0001293 -- buy price of trend line #1
        "TRENDLINE2": 0.0001292 -- buy price of trend line #2
        "TRENDLINE1_TRIGGER": "true" -- trend line #1 is triggered (when the tokens are completely sold, it becomes false and will buy again)
        "TRENDLINE2_TRIGGER": "true -- trend line #2 is triggered (with a full sale of tokens, it becomes false and will buy again)
        "LASTREBUYPRICE": "0" -- Last purchase price
        "LASTREBUYSTATUS": "false" -- enabling the algorithm
        "LASTREBUYPERCENT": "5" -- The percentage by which the last rebuy must increase in order to sell it.
	"PRICE_FALL_BUY_ALGORITM": "false" -- enabling the algorithm

		
    
## Security questions
There is a lot of deceit and fake in blockchain. What can we do to trust each other?
- Our telegram channel is open for messages between clients without moderation. Fraudsters don't do that.
- Enable key encryption with password
- Why do I have to specify wallet-key? The bot works several times per second, manually in a metamask you simply cannot sign transactions so quickly.
- Create a new wallet account
- Money only for traiding
- Run the bot on a dedicated server or virtual machine, or as a guest user on your computer Windows

## How to buy our bot
You can test our bot for free. The test key is written to the configuration files. 
When you start the bot, a wallet will be indicated on which you will need to make a payment, after that we ask you to write to us in a telegram to receive a key (license). 
### Licenses of 3 types:
Free - 1 token, delay between price checks 5 seconds.

First level (100$ per month) - 3 tokens delay by price checks 1 second.

Second level (200$ per month) - 6 tokens delay by price checks 0.1 second. 

Pay in USDT, BUSD - BEP20 0x5a8d789C4cf0fa171230cCAd008CbAb942C96EA9 and DM telegram @bigbidbotsupport


-	our team is also developing similar scripts. Send DM telegram @bigbidbotsupport	

