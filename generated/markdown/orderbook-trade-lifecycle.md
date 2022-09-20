## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit

---

## Trade Lifecycle


## Introduction

## Introduction:
Tokens on Solana are created as instances of the SPL Token Program. An Solana Program Library (SPL) token is similar 
to an ERC20 token on Ethereum.

Now, before going to Trade Lifecycle, let's first understand some terminologies related to accounts specifically global accounts:
  * **Request Queue**: This account maintains all submitted but unprocessed order placement and cancellation requests.
  
  * **Orderbook**: Speaks for itself. There's one account for bids and another for asks, but for simplicity,
  we'll refer to both of them collectively as Orderbook going forward.
  
  * **Event Queue**: Reports the list of outputs from order matching: trades, for instance
  
  * **Market**: Holds metadata about the market (e.g. important constants like tick size and references to the other accounts below)
  
  * **Base Currency Vault**: An SPL token account that holds **base currency** balances. Base currency is also known as the transaction currency and is always listed first in a currency pair quotation. 
  
  * **Quote Currency Vault**: An SPL token account that holds **quote currency** balances. Quote currency is also known as the counter currency and is always listed second in a currency pair quotation.
  
  * **Open Order Account**: To interact with the DEX, a given user must create an **Open Orders Account**. This 
    account stores the following:
    * How much of the base and quote currency the user has locked in open orders or settleable
    * A list of open orders for that user on that market


    


---
## Evaluation





##### What is the block time of Solana?  

- [ ]  4ms
- [ ]  40ms
- [x]  400ms
- [ ]  4000ms





##### Which of the following reports the list of outputs from order matching?  

- [x]  Event Queue
- [ ]  Request Queue
- [ ]  Base Currency Vault
- [ ]  Orderbook





##### What is the full name of SPL token?  

- [ ]  Serum Program Liquidity token
- [ ]  Solana Program Liquidity token
- [ ]  Serum Program Library token
- [x]  Solana Program Library token

    


---
## Trade Lifecycle

The below four steps define the lifecycle of an order withing Serum DEX

#### 1. Placing orders
A user funds an intermediary account (their OpenOrders account) from their SPL token account (wallet) and adds an 
order placement request to the Request Queue.

#### 2. Matching orders
The request is popped off of the Request Queue and processed: it’s placed on the Orderbook. Any resulting trades 
get reported in the Event Queue.

#### 3. Consuming Events
Events are popped off of the Event Queue and processed: OpenOrders account balances are updated as the result of 
the trade.

#### 4. Settlement
Users can settle free funds from their OpenOrders back to their SPL token account (wallet) at any time.


    


---
## Trade Lifecycle - Placing Orders

The trade lifecycle generally consists of four main stages: placing orders, matching orders, consuming events, and settlement.
Let's see each in detail:

## 1. Placing orders:
Users can place an order by submitting a Place Order instruction to the DEX program. To do so, they must provide the following information:
- Order details: the market, size, price, order type, side
- Their OpenOrders account on that market
- Their SPL token account for the market’s base currency (if selling) or quote currency (if buying)

Serum DEX then initiates the transfer by calculating the maximum funds required for an order ( either the size of the order if selling, or the size 
multiplied by the price if buying). It also notes the free balance for the corresponding currency in the OpenOrders 
account. Finally, it transfers the difference between the required amount and free OpenOrders balance from the SPL 
token account to either the Base Currency Vault or Quote Currency Vault. This increases the OpenOrders account's total 
balance for the corresponding currency by the amount transferred.

After the transfer, Serum DEX  increments and retrieves a sequence number from the request queue. This, along with 
the order's price, determines the new order's ID.

The Order ID is a unique identifier that is used to determine the price-time priority of an order. It is a 128-bit 
number, with the first 64 bits representing the price and the second 64 bits representing the sequence number. If 
the order is a buy order, all of the bits in the second half of the number are flipped.

Then, it adds an item to the request queue specifying the order details (size, price, order type, side). 

Finally, it adds an item representing the new order to an array in the OpenOrders account in an available slot.


    


---
## Evaluation





##### Which of the following details in not included in an order placement request?  

- [ ]  User's Open Order account on the market
- [x]  User's bank account for the market
- [ ]  User's SPL token account for the market
- [ ]  The market size, price, order type





##### Where does an order placement request gets added?  

- [x]  To the Request Queue
- [ ]  To the Event Queue
- [ ]  Both A & B
- [ ]  None of these

    


---
## Trade Lifecycle - Matching Orders

## 2. Matching orders:

This step removes requests from the Request Queue and processes them, updates orders on the Orderbook, and puts 
information about resulting trades in the Event Queue.

When an order is placed on the RequestQueue, it is transferred to the Orderbook. The Orderbook then stores the order 
details, including the specs (side, price, remaining size), the public key of the order placer's OpenOrders account, 
and the index of this order in that OpenOrders account's order array (the slot number).

 For any trades, two corresponding Fill items are added to the Event Queue. In the case of a trade, cancel, or IOC 
order that missed, Out items are added to the Event Queue.

Below is the information that Fill and the Out event contain
#### Fill
The fields contained in Fill event are 
1) Side 
2) If this side of the trade was the maker
3) Quantity paid by this counterparty (the currency is inferable from the Side: if buying, this quantity is always denominated in the quote currency)
4) Quantity received by this counterparty (if buying, this quantity is always denominated in the base currency)
5) Quantity of any fees paid (always in the quote currency)
6) Order’s ID and slot number, and the public key of the corresponding OpenOrders account


#### Out
The fields contained in Out event are
1) Side
2) Quantity unlocked
3) Quantity still locked in the order (0 in the case of a cancel or full fill, nonzero in the case of a partial fill)
4) Order’s ID and slot number, and the public key of the corresponding OpenOrders account

Note that clients can check the Orderbook account for the current state, the Event Queue account to see their trades 
(and others'), and the OpenOrders account to view their open orders (and others').


    


---
## Evaluation





##### Which of the following information is stored by both the type of event queue objects?  

- [x]  Slot number
- [x]  Order ID
- [ ]  Private key of the Open Order account
- [x]  Public key of the Open Order account





##### What are the two types of event queue objects?  

- [x]  Fill
- [ ]  Base
- [x]  Out
- [ ]  Request

    


---
## Trade Lifecycle - Consuming Events

## 3. Consuming events:
Primary job as part of this phase is to make sure that OpenOrders accounts are updated according to events emitted 
by Match Orders.

Submitting Consume Events instructions (that do anything) does involve a bit of work for the client: they have to 
read some prefix of the event log, gather and sort the public keys of the affected OpenOrders accounts, and submit 
those as writable along with other relevant global singleton accounts.

#### Processing of Event Queue
The next event in the Event Queue is considered. If the OpenOrders public key is found in the account list submitted 
by the client is writable, then continue. Otherwise, abort.

#### Fill Events
When a Fill event is found, the total balance of the OpenOrders account is decreased by the quantity paid. The total
and free balances of the OpenOrders account are increased by the quantity received. The Market's total fees paid 
counter increments by the quantity of fees paid.

#### Out Events
When the Order Event is found, the OpenOrders account's free balance increments by the quantity unlocked with 
each event. If the event's specified quantity still locked is zero, then the order is removed from the OpenOrders account's 
account list. Note that this can happen in constant time because the slot number is provided in the event. 


    


---
## Evaluation





##### Which processes take place in Fill?  

- [x]  The Open Order account's total balances are decremented by the quantity paid
- [ ]  The Open Order account's total balances are incremented by the quantity paid
- [ ]  The Open Order account's free balance increments by quantity unlocked
- [x]  The Open Order account's total and free balances are incremented by the quantity received





##### Which of the following searching algorithms is used to find  s account public key?  

- [ ]  Linear Search
- [x]  Binary Search
- [ ]  Interpolation Search
- [ ]  Exponential Search

    


---
## Trade Lifecycle - Settlement

## 4. Settlement:
The settlement instruction settles the funds of the user from their OpenOrders account to their SPL token accounts for the base and quote 
currency. The user will sign with the “owner” keypair, which is the same key that was used for placing orders. The 
runtime will then use a cross program invocation to move funds from the Base Currency Vault and Quote Currency 
Vault to the provided SPL token accounts. This is equal to the free balance amount in the OpenOrders for each 
currency.

The whole trade-lifecycle, starting from Placing the order, all the way to settlement, just takes a second or two.


    


---
## Evaluation





##### Select the correct statement(s) about settlement?  

- [ ]  Users can not settle free funds from their Open Orders back to their SPL token account at any time
- [x]  Users can settle free funds from their Open Orders back to their SPL token account at any time
- [ ]  Users can settle free funds from their Open Orders back to the Base Currency Vault at any time
- [ ]  Users can settle free funds from their Open Orders back to the Quote Currency Vault at any time





##### What is the time taken by the whole trade-lifecycle?  

- [ ]  1-2 minutes
- [ ]  1-2 hours
- [x]  1-2 seconds
- [ ]  1-2 milliseconds

    


---
## Your Info





| Label | Type | Required |
| ----------- | ----------- | ---- |
| Nick Name        | PublicShortInput   |  true    |




    


---
## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit
* Serum's new Asset-Agnostic orderbook - https://medium.com/serum-stories/bonfida-delivers-a-new-core-for-serum-asset-agnostic-orderbook-serum-stories-11-3d1b3c03d25c
* 
    
