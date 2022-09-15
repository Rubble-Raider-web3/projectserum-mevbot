## Introduction
Order Book is a crucial tool for traders, as it allows them to see the current buy and sell activity for a 
particular asset. This helps them make more informed decisions about their trades, and ultimately increase their 
chances of success.

The order book is a running list of all the open orders that have been placed by traders who haven't been matched yet. 
For every entry in the order book, there is at least one other trader with an open order at that exact price. The top of 
the list in red are the open asks, or the prices that sellers are asking for, and below in green are the open bids, or 
prices that buyers are willing to pay.

Since these are all open orders, they give us an insight into what prices people think they can get their orders filled at.

By taking all of this information into account, traders can make wiser decisions about their  trades and improve their 
chances of making a successful trade.

For a successful trade the sell and the buy price needs to overlap. For example, if someone places an open order on an 
exchange to buy Ethereum for 2,000 USD, someone else on the exchange will need to agree to sell their Ethereum at the 
same price of 2,000 USD.

An imbalance in buy or sell orders on an order book can show where the market might be headed. When there are lots of 
buy orders at a particular level, it could signal that there's support at that level. On the other hand, if there are 
lots of sell orders, it might be an area of resistance. But it's important to keep in mind that these aren't always 
reliable signals on their own.

The order book trading instrument is designed to promote transparency on exchanges. It indicates prices that each 
buyer and seller are willing to accept, making market manipulation more difficult.

## Important Terms
### Bid
The bid is the price at which the buyer is willing to purchase the asset

### Ask
The Ask is the price that the seller is willing to sell the asset for

### Amount  
Number of tokens(asset) you plan to buy or sell

### Price
The "$" (price) at which you plan to buy or see a certain token or asset

### Market Order
A market order is simply an order to buy or sell an asset at its current market price. For example, if the current market 
price for Bitcoin is $2,000, and you place a buy order at that price, the trade will go through and you'll purchase the 
Bitcoin for $2,000.


### Limit Order
A limit order, as opposed to a market order, is an automated order to buy or sell a financial instrument at a predetermined 
price. So, for example, if the current market price of Bitcoin is $2,000 but your analysis shows that it would be a 
better buy at $1,950, you would place a limit order at $1,950. The trade will not go through until the Bitcoin can be 
bought at that price.

## Buy and Sell

### The Buy Side
All "open buy orders" that are below the last traded price make up the buy side. When a buyer makes an offer to purchase 
something at a specific price, this is called a "bid." In other words, the bid represents the trader's interest in buying 
a certain number of units that are owned by someone else, usually at a price that is lower than what was paid for them 
originally.

Once a bid is matched with a corresponding sell order, the trade can be executed.

### The Sell Side
The sell side of the market contains all open sell orders that are above the last traded price. This price is called the 
"ask." It means, "I am asking a certain price to sell X units I own"

### Bid vs Ask
For every asset traded, there is a buyer and a seller, and a “bid” and “ask” price. The bid is the price at which the 
buyer is willing to purchase the asset, while the ask is the price that the seller is willing to sell the asset for.

There will usually be a gap between the bid and ask price called a “spread” or “bid/ask spread.” The bid/ask spread represents 
the difference between the bid and ask prices, and is dependent on the volume of trades being submitted. For example, if there 
is a large volume of orders in a asset’s order book, the spread will be narrower than if there are fewer orders.

## Example
In this example we will understand how Order Book works based on price and time.

In this example we have three sellers and three buyers. 

Here time t1 < t2 < t3 i.e. order placed at t1 happens first, then t2 and then the t3 order is placed.

### Seller Orders
1) Seller 1 at t1, asks for price $9 for each token and wants to sell 5 tokens
2) Seller 2 at t2, asks for price $7 for each token and wants to sell 3 tokens
3) Seller 3 at t3, asks for price $8 for each token and wants to sell 9 tokens

### Buyer Orders
1) Buyer 1 at t1, bids at price $6 for 8 tokens
2) Buyer 2 at t2, bids at price $8 for 5 tokens
3) Buyer 3 at t2, bids at price $8 for 3 tokens

Here Seller-1 asks for too high of a price i.e. $9 for each token and no one wants to buy at this high price, hence
their order is not matched.

Seller-2, asks for $7 as the price per token, and Buyer-2 and Buyer-3 both are okay with buying at $8/token. Here Buyer-2 is given 
preference as their order was placed first.


![Example](https://raw.githubusercontent.com/DoDAO-io/dodao-projectserum-guides/main/images/how_clob_works.png)



## References
https://www.coindesk.com/learn/crypto-trading-101-how-to-read-an-exchange-order-book/
https://academy.shrimpy.io/post/what-is-an-exchange-order-book
https://medium.com/stabletrade/understanding-order-books-orders-b297ea51acff
https://www.cryptoglobe.com/latest/2020/02/breaking-down-how-crypto-exchanges-and-order-books-work/
