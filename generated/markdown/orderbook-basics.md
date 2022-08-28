## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit

---

## Intro to Central Limit Order Book


## Intro & Terms

Before Knowing the CLOB in detail one must understand the following terms:

1. `Limit order`: You can purchase or sell a stock using a limit order at the price you specify or a higher one. You can make a purchase or sell order at the price you want by using a limit order.

2. `Automated Market Maker (AMM)`: AMMs allow for the unrestricted trading of digital assets utilising pre-established algorithms placed inside of liquidity pools. Uniswap (a DEX) uses AMM to trade on its platform.

3. `Request for Quote (RFQ)`: When an organisation needs supplies or services they contact potential vendors to send their price quotations. This helps suppliers stand a chance to get an order. 

4. `Invitation for Bid (IFB)`: A business or organisation issues an invitation to bid to request proposals from vendors for a particular good or service that an organisation is aware it wants or needs.

5. `Market Liquidity (ML)`: It refers to how quickly or easily an asset may be turned into cash without affecting its market value.

6. `Price discovery (PD)`: Price discovery is the process of determining an asset's value through exchanges between buyers and sellers in the market at a cost that both sides deem tolerable.

7. `Liquidity Providers (LPs)`: It is the duty of this individual or organisation to support trade on decentralised exchanges by contributing money to the liquidity pools and profiting from their investment.

8. `Market Fragmentation`: Market fragmentation is the division of a homogeneous market into numerous groups, each of which has its own unique needs, demands, and preferences.


    


---
## Serum-Deep Dive

### Introduction to Serum
Built on the Solana network, Serum is a very effective decentralised exchange that is also quite scalable and therefore widely accepted. Serum's platform uses Central Limit Order Book (CLOB) rather than Automated Market Makers (used by others) to facilitate efficient and quick trading, which is the key distinction between Serum and other well-known DEXs. 

Serum's built-in functionality is enhanced by its on-chain CLOB, a commonly used mechanism in traditional finance. Additionally, Serum makes it permissionless and decentralised, enabling users to communicate with a smart contract directly to execute trades from an order book without the need for a middleman.

### Central Limit Order Book 
The CLOB model, which enables users to clearly match their other with others on a "price-time-priority" basis, is one of Serum's most helpful design elements. Customers could opt to cross this bid-ask spread and have their order promptly filled in this model's order book where the market price is represented by the highest bid and lowest ask.

High levels of transparency offered by the CLOB model allow customers to monitor market depth in real time. Although CLOB has very powerful features, integrating it into an on-chain system is exceedingly difficult. The key reasons for this difficulty are order books' high throughput and cheap execution costs. Therefore, low gas costs and rapid transactions are necessary for a CLOB to operate on-chain.

Many of the first DEX protocols were conscious of these challenges and made an effort to replace the traditional CLOB paradigm with an AMM. By employing their AMM technology, Uniswap's Defi users were able to complete transactions rapidly and affordably.


    


---
## Orderbook vs AMM

Orderbook and Automated market maker are two ost popular trading models. They primarly differentiate on the basis on structure and underlying working principle.

### Structural Differences 

An order book works on the principle of FIFO- First In First Out. An algorithm, in the Order Book, organises the list of traders who submitted their intent to transact to buy or to sell and subsequently organises them in a database i.e. order book. This database is then made public to all the participating traders.

AMM is a trading model that is automated.The main distinction between the two is that whereas AMM employs two-sided liquidity pools in an order book where both buyers and sellers serve as the liquidity custodian. Given its support for intermediary intervention and manual components, the Order Book trading model is popular among centralised exchanges, whereas AMM is the fundamental protocol utilised by decentralised exchanges to do away with middlemen when trading crypto assets.

### Working Principle Differences

The orderbook principle comes into play when  the exchange encounters overlapping orders in the order book especially when there are multiple requests regarding the same value and quantity of a traded asset, in such cases the exchange that uses an order book concept steps into matching these orders manually using the FIFO. 

AMM gives consumers the ability to exchange their assets directly between themselves, without the use of a middleman. While an AMM is constantly active, an order book only activates when there are overlapping transactions. To compute the exchange rate, this AMM uses the Constant Market Maker Model: The number of X-type cryptos multiplied by the number of Y-type cryptos equals K, or X * Y.


    


---
## Advantages of CLOB

Some of advantages of CLOB model are:

* Users can execute orders using smart contracts in Serum's fully decentralised, on-chain order book while keeping pricing and order size flexibility.

* Similar to a standard CLOB, users have the option to specify the price, amount, and direction of their transactions. This technique provides high liquidity by matching orders based on price, time, and priority.

* Serum enhances composability as it can be used by other protocols in the area to bootstrap liquidity and provide matching services.

* The fast throughput and low transaction costs of the Solana network assist the Serum order book in minimising capital inefficiencies and liquidity segmentation.

* Serum's CLOB is built to be asset agnostic and has additional strong features like cross-chain swaps. It may create an order book that corresponds to any trading product offered by Solana, including options and futures (e.g. ETH and BTC).

* Due to Serum's flexible protocol, a wide range of participants and applications can share middleware in one place. This is made possible by the backend matching engine's versatility, which allows it to be extended to almost any financial or non-financial item.

* Other applications can connect to Serum and leverage its infrastructure to build their own projects thanks to on-chain CLOB, such as a trading application that utilises Serum's liquidity.


    


---
## Future Prospects of CLOB

### Providing More Financial Services
A wide range of intricate financial services may be created on top of the DEX, and they all interact with one another to add value. DeFi's modularity and the synergies produced by a system of projects connected first to Serum DEX and then, possibly, to one another benefit users greatly.

### Modified Borrowing and Lending
A composable on-chain CLOB can be used to create a borrow-lending protocol with specialised infrastructure for capital efficiency and leveraged trading. One click might enable in-pool trading to exchange directly and margin trading via borrow-lending iterations. Due to the permissionless nature of DeFi and the latency and expense of Solana, such on-chain capabilities are possible.

### Diversification beyond DEX
Everything bottoms out to the order book and DEX on the chain, regardless of the system's configuration. Stakeholders in the social networking, gaming, and travel industries can all gain from advancing and collaborating with the Serum DEX.

This ought to provide a crystal-clear and upbeat image of Serum DEX's potential as the matching engine powering Solana-based projects that make use of the DEX's matching service, liquidity, and price data.


    


---
## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit
    
