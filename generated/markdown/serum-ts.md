## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit

---

## Serum Typescript


## @project-serum/borsch, @project-serum/common



Serum is a Decentralized exchange on the solana blockchain. It is built with the motive of bringing reducing the transaction costs and improving the speed 
of functioning of decentralized exchanges. It is a monorepo with many packages. In this guide we will be taking a look at some of them.

### borsch-ts:
Borsch is a serialization format used to convert human readable code to binary code and the respective binary code back to human 
interpretable code. It stands for "Binary Object Representation Serializer for Hashing". While there are other such serialization formats 
like Serde and bincode, the team behind borsch argues that borsch is the first attempt of its kind which is meant to be used in "security crucial 
projects as it priotizes speed, consistency, safety and comes with strict specifications for usage."
|
**Consistent:** 
Consistent means there is a one-to-one mapping between objects and their binary representations, meaning that the binary representation of each object is unique. 
This is extremely useful for applications that use binary representation to compute hash.
**Specifications:** 
Borsh comes with a full specification that can be used for implementations in other languages.
**Safety:** 
Borsh implementations use safe coding practices. In Rust, Borsh uses almost only safe code, with a few exceptions.
**Speed:** 
Borsh achieves high performance by opting out from Serde which makes it faster than bincode in some cases, which also reduces the code size.

**Features:**
 Opting out from Serde allows borsh to have some features that currently unavailable for serde-compatible serializers. 
 Currently borsch supports two features,
  borsh_init and borsh_skip (the former one not available in Serde).
 borsh_init allows to immedeately run an initialization function after deserialization of an object. 
 This adds a lot of convenience for objects that are architectured to be used as strictly immutable. 

 borsh_skip allows to skip serializing/deserializing fields, assuming they implement Default trait, similary to #[serde(skip)].

 ### common-ts:
 Contains some common utilities to interact with project-serum. 
 Contains methods like
 `getMultipleSolanaAccounts()` - returns an array of account details of solana accounts based on the array of public keys passed into the function.
 `createMint()` - function allows to create mints based on the values of provider, authority and value passed in using. 
 `createMintInstructions()` - allows to set instructions for the createMint function. 
 The `createMint()` function calls the `createMintInstructions()` to createMint based on the instructions passed in.
 `createTokenAccount()` - lets you create a token account, common-ts also has `createMintAndVault()` function.
 Also includes getter methods which let the user query mint info, account info, owned token accounts, and also functions to parse data like pareMintData etc.,.


    


---
## Evaluation





##### Why is there a need for borch when there are other serialization formats available?  

- [x]  It is sometimes faster than its counterparts
- [x]  The serialized values are unique, no two objects can point to the same serialization
- [x]  Implementation in other languages is made easier
- [ ]  It is always faster than its counterparts





##### What is the significance of borsh opting out of Serde?  

- [ ]  Allows implementation of a method to skip serializing fields, which is not currently available in serde-compatible serializers
- [x]  Allows borsh to function more efficiently
- [x]  Allows borsh to have methods not supported by other serde-compatible serializers
- [x]  Allows implementation of a method to run initialization function immedeately after deserialization of an object which is not currently possible in serde-compatible serializers





##### common-ts provides the user methods to do which of these?  

- [ ]  To query account information
- [ ]  To parse various data
- [ ]  createMint, createMintAndVault
- [x]  All of these





##### what is the use of borsh, what does it stand for?  

- [ ]  To query accounts information on solana
- [ ]  To serialize data into a storable, transferrable format (byte code) and vice-versa
- [ ]  Borsch stands for - Binary Object Representation Serializer for Hashing
- [x]  Both C and D

    


---
## @project-serum/serum, @project-serum/pool



### @project-serum/serum:
Its a JavaScript client library for interacting with the Project Serum DEX.

Like any other client library, @project-serum/serum contains various methods for to make it easy for the developer to write applications on top of serum DEX. 
Let us take an example of market.ts,
market.load() allows the user to connect to the market with specified market address.
market.loadBids() allows user to load(retrieve) the bids in the given connection.
market.loadAsks() allows the user to load asks in a given connection.
market.placeOrder()  allows the user to place an order, specifying the buyer, seller, the amount and the type of order.
market.cancelOrder() cancels the order.
market.loadOrdersForOwner() loads the open orders of the owner.
market.settleFunds() used to settle the funds of an order.
market.findOpenOrdersAccountsForOwner() The open orders and accounts associated can be retireved by this method.

queue.ts contains methods to decode EventQueues, RequestQueues and methods that deal with different implications of them, like 
decodeEventsSince(), which decodes the event queues of all events since a certain time period.
|
It contains other modules like token_and_markets.ts, 
fees.ts :
contains methods to retireve feeRates (getFeeRates()) and feeTiers (getFreeTiers()). 
and, error.ts:
contains errors to be thrown under various circumstances.
|
It can be installed by,
```
  Using npm:
  npm install @solana/web3.js @project-serum/serum
  
  Using yarn:
  yarn add @solana/web3.js @project-serum/serum
```
### @project-serum/pool:

It is a client library for interacting with project-serum pools.
Offers methods like,
getPoolBasket() fetches the current pool basket (the quantity of each token needed to create N pool tokens or the quantity of each token received for redeeming N pool tokens).
loadPoolInfo() Fetches the pool state. Once fetched, the data can be decoded using decodePoolState().
Pool tokens can be created and reedemed by sending transactions. 
transactions can be executed by writing the code and passing it as parameter inside the poolTransactions.execute() function.
 
To install,

```
Using npm:
npm install @solana/web3.js @project-serum/pool

Using yarn:
yarn add @solana/web3.js @project-serum/pool
``` 


    


---
## Evaluation





##### What are some of the different modules in @project-serum/serum?  

- [x]  market.ts
- [x]  queue.ts
- [x]  error.ts
- [ ]  pool.ts





##### Which of these is the right definition of the given functions?  

- [ ]  market.loadOrdersForOwner() loads the possible orders the owner might be interested in.
- [ ]  market.loadAsks() allows the user to ask in a given connection.
- [x]  market.loadBids() allows user to load(retrieve) the bids in the given connection
- [x]  market.load() allows the user to connect to the market with specified market address.





##### fees.ts provides methods for which of these actions?  

- [ ]  To retrieve feeRates
- [ ]  To decodeEventQueues
- [ ]  To retrieve feeTiers
- [x]  Both A and C





##### can pool package can be utilized to create and redeem pool tokens? how?  

- [ ]  No, it strictly deals with pool info.
- [x]  Yes, by sending a transaction.

    


---
## @project-serum/spl-token-swap, @project-serum/tokens



### @project-serum/spl-token-swap (previously, @project-serum/swap):

Contains modules to deal with various phases of swapping in project project-serum.

**instructions.ts:**
Deals with the getting/setting of instructions on transfers, deposits and so on in project-serum.
Contains instructions (structs) for each of the components of swapping in serum, like:
TokenSwapLayoutLegacyV0
TokenSwapLayoutV1
FeeLayout

also, contains methods 
getLayoutForProgramId() returns the tokenswap layout used in the program with given id.
getCreateInitSwapInstructionV2Layout() depending on curveType, returns a InitSwapInstructionV2Layout struct.
Contains methods like, depositInstruction, withdrawInstruction etc., to set instructions for each of the actions.
parseTokenAccount() parses the account data.


**pools.ts**
Deals with pools in case of swapping functions.
Contains getter methods which fetch 
publicKey, programId, isLatest, holdingAccounts, tokenMints, feeAccount, CachedMintAccount, CachedTokenAccount, 
methods to add/remove LiquidityTransaction (makeAdd/RemoveLiquidityTransaction), SingleSidedLiquidityTransaction etc.,.
Also has functions to initializePool, removeLiquidity, addLiquidity, sendTransaction, approveTransfer etc., 

**Types.ts**
This smart-contract contains the interfaces and enums (CurveType(enum), TokenAccount, PoolConfig, PoolOptions) to be utilized by the rest of the package.

**utils.ts**
Contains functions to perform math specific to the working of this package.( divideBnToNumber(), getTokenMultiplierFromDecimals()).

### @project-serum/tokens:
This package contains json and ts files that specify the token addresses that are used in project-serum (mainnet, testnet and devnet) 
and their type respectively(MAINET/ DEVNET/ TESTNET_TOKENS).


    


---
## Evaluation





##### instructions.ts contains provision to perform which of these?  

- [ ]  to set instructions about the market connection
- [x]  contains structs for TokenSwapLayout(s).
- [x]  sets instructions for the swapLayout based on the CurveType
- [x]  set instructions for withdraw, deposit functions.





##### types.ts contains?  

- [x]  CurveType as enums
- [x]  PoolConfig and PoolOptions as interfaces
- [x]  TokenAccount interface
- [ ]  PoolConfig enum





##### functions dealing with adding and removing of liquidity transactions in spl-token-swap are part of?  

- [ ]  instructions.ts
- [x]  pools.ts
- [ ]  market.ts
- [ ]  Both A and B





##### tokens package contains data related to tokens in serum, what data is it exactly?  

- [ ]  Determines the token addresses used in mainnet, testnet and devnet.
- [x]  Determines token addresses used in mainnet and testnet alone.
- [ ]  Contains enums to determine if the given token address is used in mainnet, testnet or devnet.
- [ ]  Contains methods to get and set the value of each of token addresses in the market.

    


---
## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit
* Serum's new Asset-Agnostic orderbook - https://medium.com/serum-stories/bonfida-delivers-a-new-core-for-serum-asset-agnostic-orderbook-serum-stories-11-3d1b3c03d25c
* 
    
