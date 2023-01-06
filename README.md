# TA Trader

![IMAGE_ALT](ta-trader.jpg)

TA Trader is a python bot that can automate day trades of stocks and foreign exchange currencies. It starts with the daily atr filter. This is a python script that reads in a csv file with 1500 stock tickers. It then queries daily open, high, low, and close price data for the last 15 days from the Alpaca Trade api which it recieves as a Pandas dataframe. Alpaca is a stock broker that provides a public api to it's users. It then passes this data into the TA Lib library which calculates the Average Trading Range(ATR). This value will be used to determine whether to trade the stock at all and used to determine a closing price target if a position is opened on that stock. Then a filter will add the stock to an output list if the stock has a high enough ATR, a low enough price, and a high enough volume. This list is output to another csv file which is read in by the main trading script.

 The main trading script is run while the market is trading live and constantly monitors each stock in the list. There are two loops, the main trading loop, and the loop that iterates over each stock. At the beginning of the main trading loop, it checks the time, the total portolio value, and the current portfolio holdings. The time and portfolio value can be usesd to automatically shut off the script, like right before the market closes, or if the bot is losing too much money. The portfolio holdings are used so when it's looping over the stocks it will know for each stock whether  it already owns that stock. Then it starts looping over the stocks. For each stock it queries price data down to the minute. It passes that data into TA Lib to calcluate various technical indicators. Then it passes that data to a signal function. This function will either return a buy, sell, close, or null signal. If the stock is not already in the portfolio, the function will check the indicators. If they indicate the price of the stock will go up, it will buy, if they indicate the price will go down, it will short sell. If the stock is already in the portfolio, it check the current price and the price the stock was bought at. If the current profit or loss is at a certain percentage of the ATR, the script will close the position. if none of these conditions are met, a null signal is sent. This signal is used to generate a network request that is sent to the Alpaca Trade api along with the api key for my account and this way the script can make trading decisions automatically on behalf of my account. If a null signal is revieved, no action is taken. I also adapted the script to trade foreign exchange currencies using Oanda as the broker and connecting to the Oanda api. 

Languages: Python

Libraries and Frameworks: Pandas, Numpy, TA Lib

Apis: Alpaca Trade, Oanda Trade, Yahoo Finance