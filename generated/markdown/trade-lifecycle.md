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
The Serum DEX is a program on the Solana blockchain. Solana has block times of 400ms, and can currently process roughly 50,000 transactions per second.
This allows Serum to handle hundreds of orders per second per market.
Tokens on Solana are instrumented as instances of the SPL Token Program. An Solana Program Library (SPL) token on Solana is analogous to an ERC20 token on Ethereum.
Now, before going to Trade Lifecycle, let's first understand some terminologies related to accounts specifically global accounts:
  * **Request Queue**: This account maintains all submitted but unprocessed order placement and cancellation requests.
  * **Orderbook**: Speaks for itself. There's one account for bids and another for asks, but for simplicity,
  we'll refer to both of them collectively as Orderbook going forward.
  * **Event Queue**: Reports the list of outputs from order matching: trades, for instance
  * **Market**: Holds metadata about the market (e.g. important constants like tick size and references to the other accounts below)
  * **Base Currency Vault**: An SPL token account that holds **base currency** balances. Base currency is also known as the transaction currency and is always listed first in a currency pair quotation. 
  * **Quote Currency Vault**: An SPL token account that holds **quote currency** balances. Quote currency is also known as the counter currency and is always listed second in a currency pair quotation.
  * **Open Order Account**: It is like an accounting system that calculates a user's position for a given currency pair quotation. For example, if a currency pair quotation has USDC (base currency) and ETH (quote currency) in a market, then a user trade in this market might have a certain amount of each currency stored in the order book or free for withdrawal. This system manages the accounting differences between USDC (base currency) and ETH (quote currency) for each user. Open Order Accounts are created by the user but maintained by protocol or DEX.

And the user-specific accounts: to interact with the DEX, a given user must create an **Open Orders Account**. This account stores the following:
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
## Trade Lifecycle - Placing Orders

The trade lifecycle generally consists of four main stages: placing orders, matching orders, consuming events, and settlement.
Let's see each in detail:

## 1. Placing orders:
A user funds an intermediary account (their Open Order Account) from their SPL token account (wallet) and adds an order placement request to the Request Queue.
### Necessary steps required in placing successful orders:
  * 1. A user submits a **Place Order** instruction to the DEX program, passing:
    * Order details: the market size, price, order type, side
    * Their Open Order Account on that market
    * Their SPL token account for the market's base currency (if selling) or quote currency (if buying)
  * 2. The **DEX program** then:
    -Transfers funds:
      * Determines the max funds required for this order (the market size if selling, or the market size * price if buying)
      * Notes the free balance for the corresponding currency indicated in the Open Order Account
      * Transfers the difference between the required amount and free Open Orders balance from the SPL token account to the Base Currency Vault or Quote Currency Vault.
      * Increments the Open Order Account's previous total balance for the corresponding currency by the extra amount transferred (i.e the residual amount from the last transaction).
    - Increments and retrieves a sequence number from the Request Queue: this, along with the order's price, determines the new order's ID.
    - Adds an item to the Request Queue specifying the order details (market size, price, order type, side)
    - Adds an item representing the new order to an array in the Open Order Account in an available slot and it looks like:
      *[ Order1(size, price, order type, side), Order2(size, price, order type, side), Order3(size, price, order type, side), and so on... ]


    


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
  * The request is popped off the Request Queue and processed, and then placed on the Orderbook. Any resulting trades get reported in the Event Queue.
  * In a Match Orders transaction, clients (or crank turners: who perform the order matching process) provide a limit parameter,
  which specifies the number of requests to process from the Request Queue.
  -Each request is processed as follows:
    * It is popped off of the RequestQueue account
    * The corresponding cancellation or order placement is made on the Orderbook (the order book consists of one tree per side, and 
    so finding the relevant part of the appropriate tree is made easier by the comparison property of Order IDs mentioned earlier)
    * In the case of a new order that stays on the book, the Orderbook stores details about it, including the obvious specs (side, price, remaining size),
    the public key of the order placer's Open Order account, and the index of this order in that Open Order account's order array (the slot number).
    * Order types like **IOC**(Immediate Or Cancel order) and **Post-Only** are enforced. Let's see what does IOC and Post-Only means:-
      * **IOC**- An immediate or cancel order (IOC) is an order that is mostly filled within the next few seconds and then the remainder is canceled.
      * **Post-Only**- The 'Post Only' order condition means that your order can only be posted when it can enter the Order Book. In other words, this condition is designed to ensure that you will receive a maker rebate. If your order can't enter the Order Book, it will be immediately canceled.
    * For any trades, two corresponding Fill items are added to the Event Queue
    * In the case of a trade, cancel, or IOC order that is missed, Out items, are added to the Event Queue
  - The two types of event queue objects store the following information, which will be useful for the Consume Events step:
    - Fill:
      * Side
      * If this side of the trade was the maker
      * The quantity paid by this counterparty (the currency is inferable from the Side: if buying, this quantity is always denominated in the quote currency)
      * Quantity received by this counterparty (if buying, this quantity is always denominated in the base currency)
      * Quantity of any fees paid (always in the quote currency)
    - Out:
      * Side
      * Quantity unlocked
      * Quantity still locked in the order (0 in the case of cancel or full fill, nonzero in the case of partial fill)
  - And they both also store: the order's ID and slot number, the public key of the corresponding Open Order account


    


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
  * Events are popped off of the Event Queue and processed. This usually results in Open Order account balances being updated as a result of the trade.
  ### Necessary steps required in successfully consuming events:
    - The next event in the Event Queue is considered. If its Open Order account public key is found (via binary search - hence the sorting requirement)
    in the account list submitted by the client as writable, then continue. Otherwise, abort.
    - That event is popped off the queue and processed.
      - Fill:
        * The Open Order account's total balances are decremented by the quantity paid
        * The Open Order account's total and free balances are incremented by the quantity received
        * The Market's total fees paid counter increments by the number of fees paid
      - Out:
        * The Open Order account's free balance increments by quantity unlocked
        * If the event's specified quantity still locked is zero, then remove the order from the Open Order account's account list.
        Note that this can happen in constant time because the slot number is provided in the event.


    


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
  * Users can settle free funds from their Open Orders back to their SPL token account at any time.
  - Step required in the settlement:
    * The user passes their Open Orders account and their SPL token accounts for the base and quotes currency,
    signs with the “owner” of all of them (the same key pair that signed for placing orders using these same accounts),
    and the runtime does the following:
      * Using a cross-program invocation to the SPL token program moves funds from the Base Currency Vault 
      and Quote Currency Vault to the provided SPL token accounts equal to the free balance amount in the Open Orders for each currency.

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
    
