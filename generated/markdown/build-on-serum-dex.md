## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit

---

## Build on Serum's Dex


## Introduction

Serum is a decentralized exchange that anyone in the world can open an account on almost instantly. All you need is an
internet connection and computer, and with some crypto assets you're ready to trade with anyone on the planet. Serum is
a decentralized open source project that everyone can contribute to building. Project Serum is built by the Serum
Foundation - the foundation is a group of experts in cryptocurrencies trading and decentralized finance.

Serum's JavaScript Library `@project-serum/serum` is useful for interacting with the DEX Smart Contracts. `@project-serum/serum`
makes it easier to program on top of Serum DEX by abstracting out complex logic, which makes it simpler for other applications
to integrate or build on top of Serum DEX.

DeFi projects often face the challenge of attracting enough liquidity to keep the protocol/product running. However, by
building on top of Serum's DEX, applications can tap into a deep pool of liquidity. Serum's DEX enables many different
types of applications, including advanced financial marketplaces and borrow lending.

This guide explains how you can call various methods of `@project-serum/serum` to
1) Fetch the market data
2) Fetch all the order book data
3) Fetch open order of a particular account
4) Place a new order on order book
5) Cancel an order
6) Retrieve filled orders
7) Settle the funds within various user accounts used in DEX


    


---
## Evaluation





##### What are the benefits of using `@project-serum/serum` library?  

- [ ]  All the complex code to interact with Serum DEX is abstracted out into easy to use Javascript library
- [ ]  It makes it easy for the programmers to interact with Serum DEX.
- [ ]  Different UI applications can use `@project-serum/serum` and build application on top of Serum DEX.
- [x]  All the above





##### Which of these operations can be performed by using `@project-serum/serum` library?  

- [x]  Fetch open order of a particular account
- [x]  Place a new order on order book
- [ ]  Interact with Solidity Smart Contracts
- [x]  Retrieve filled orders

    


---
## Pull Market and Order Book information

You would need market address and program address for connecting to the Serum DEX. Market and program id can be found
from [https://github.com/project-serum/serum-js/blob/master/src/markets.json](https://github.com/project-serum/serum-js/blob/master/src/markets.json)

Below code explains how you can load a market and its data
```javascript
  import { Account, Connection, PublicKey } from '@solana/web3.js';
  import { Market } from '@project-serum/serum';
  
  let connection = new Connection('https://testnet.solana.com');
  let marketAddress = new PublicKey('...');
  let programAddress = new PublicKey("...");
  let market = await Market.load(connection, marketAddress, {}, programAddress);
  
  // Fetching orderbooks
  let bids = await market.loadBids(connection);
  let asks = await market.loadAsks(connection);
  // L2 orderbook data
  for (let [price, size] of bids.getL2(20)) {
    console.log(price, size);
  }
  // Full orderbook data
  for (let order of asks) {
    console.log(
    order.orderId,
    order.price,
    order.size,
    order.side, // 'buy' or 'sell'
    );
  }
```


    


---
## Placing an order

Users can place an order by submitting a Place Order instruction to the DEX program. To do so, they must provide the 
following information:
- Order details: the market, size, price, order type, side
- Their OpenOrders account on that market
- Their SPL token account for the market’s base currency (if selling) or quote currency (if buying)

`@project-serum/serum` simplifies the placement of new order and saves us from getting into the complexity of passing
parameters like open order account etc.

```javascript
  // Placing orders
  let owner = new Account('...');
  let payer = new PublicKey('...'); // spl-token account
  await market.placeOrder(connection, {
    owner,
    payer,
    side: 'buy', // 'buy' or 'sell'
    price: 123.45,
    size: 17.0,
    orderType: 'limit', // 'limit', 'ioc', 'postOnly'
  });
  ```


    


---
## Cancelling and order

Cancellation takes in an order (which includes the slot number and Order ID) adds an event to cancel it to the Event Queue.

The Order ID is a unique identifier that is used to determine the price-time priority of an order. It is a 128-bit
number, with the first 64 bits representing the price and the second 64 bits representing the sequence number. If
the order is a buy order, all of the bits in the second half of the number are flipped.


```javascript
  // Retrieving open orders by owner
  let myOrders = await market.loadOrdersForOwner(connection, owner.publicKey);
  
  // Cancelling orders
  for (let order of myOrders) {
    await market.cancelOrder(connection, owner, order);
  }    
```


    


---
## Retrieving filled orders and Settling funds

After the trade has been made the "Fill" events are fired. When a Fill event is found, the total balance of the OpenOrders account 
is decreased by the quantity paid. The total and free balances of the OpenOrders account are increased by the quantity received. 
The Market's total fees paid counter increments by the quantity of fees paid.

The settlement instruction settles the funds of the user from their OpenOrders account to their SPL token accounts for the base and quote 
currency. The user will sign with the “owner” keypair, which is the same key that was used for placing orders. The 
runtime will then use a cross program invocation to move funds from the Base Currency Vault and Quote Currency 
Vault to the provided SPL token accounts. This is equal to the free balance amount in the OpenOrders for each 
currency.

```javascript
// Retrieving fills
for (let fill of await market.loadFills(connection)) {
  console.log(fill.orderId, fill.price, fill.size, fill.side);
}

// Settle funds
for (let openOrders of await market.findOpenOrdersAccountsForOwner(
  connection,
  owner.publicKey,
)) {
  if (openOrders.baseTokenFree > 0 || openOrders.quoteTokenFree > 0) {
    // spl-token accounts to which to send the proceeds from trades
    let baseTokenAccount = new PublicKey('...');
    let quoteTokenAccount = new PublicKey('...');

    await market.settleFunds(
      connection,
      owner,
      openOrders,
      baseTokenAccount,
      quoteTokenAccount,
    );
  }
}
```


    


---
## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit
    
