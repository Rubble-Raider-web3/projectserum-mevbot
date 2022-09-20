## Header
This is the course header. This will be added on top of every page. Go to [DoDAO.io](https://www.dodao.io) to know more.

## References
* All links - https://github.com/project-serum/awesome-serum
* Pools - https://docs.google.com/document/d/1lmMZRKkxMFOtGOEZOFEKYL7syqv-4QT87F0o55fc35Y/edit
* Serum DEX - https://docs.google.com/document/d/1isGJES4jzQutI0GtQGuqtrBUqeHxl_xJNXdtOv4SdII/edit

---

## Developer Resources


## Introduction

## What is Solana?
Solana is a fast, secure, and censorship-resistant blockchain providing the open infrastructure required for global adoption. 
It uses a proof-of-stake mechanism as well as a "proof-of-history" mechanism to achieve consensus. 
The latter enables the network to function more quickly because nodes don't need to communicate in order to validate a block.
As a decentralized clock, this design is described in the Solana whitepaper. 
Furthermore, proof of history allows network participants to have greater assurance that an event occurred at a specific time. 
An example of proof of history would be if someone took a picture of today's newspaper with the date and time included
so that it could be used to verify the authenticity of the newspaper down the line.

## Specifications
Solana is one of the few blockchains out there that is designed to be able to handle a lot of users and devices 
without sacrificing decentralization or security. Its specs include: 
  - supporting 50k transactions per second
  - block times of approximately 400ms
  - network fees of $0.0001 per transaction
  - no sharding is required

With these specs, Solana can handle hundreds of orders per second per market. 
Plus, it's built to scale with Moore's Law, doubling in capacity around every two years with hardware and processing improvements. 
The current foreseeable roadmap envisions 1m TPS and 150ms block times.

## Why Solana?
The Solana blockchain was selected to be the foundation for Serum for a few key reasons. 
Firstly, Solana is known for being a high-performing blockchain with low transaction latency. 
Secondly, the team behind Serum saw an opportunity for DeFi to grow immensely and cover a value of $10T. 
In order to achieve such ambitious goals, they knew they needed to build on a blockchain that could support high throughput 
and low fees- which is exactly what Solana offers.

## Some useful links
- [Solana Program Library](https://github.com/solana-labs/solana-program-library)
- [Solana Web3 SDK](https://github.com/solana-labs/solana-web3.js)
- [Solana Explorer](https://github.com/solana-labs/solana/tree/master/explorer)
- [Solana Hello World](https://github.com/solana-labs/example-helloworld)
- [Solana Technical Documentation](https://docs.solana.com/)
- [Solana Discord](https://solana.com/discord)
- Solana Wormhole - (bidirectional, trustless ERC-20 ⇄ SPL token bridge)
  * [GitHub repository](https://github.com/certusone/wormhole)
- [Solnet SDK (.NET integration for Solana)](https://github.com/bmresearch/Solnet)
- [NFT example](https://spl.solana.com/token#example-create-a-non-fungible-token)
- [Solana Go](https://github.com/dfuse-io/solana-go)


    


---
## Evaluation





##### What is the block time of Solana?  

- [ ]  4ms
- [ ]  40ms
- [x]  400ms
- [ ]  4000ms





##### Which of the following consensus mechanism(s) is used by Solana?  

- [x]  proof-of-stake
- [ ]  proof-of-network
- [x]  proof-of-history
- [ ]  proof-of-blockchain





##### Select the correct specifications of Solana.  

- [ ]  High network fees per transaction
- [ ]  Sharding is required
- [x]  No sharding is required
- [x]  Low transaction latency





##### What is the typical network fees per transaction on Solana?  

- [ ]  $0.1
- [x]  $0.0001
- [ ]  $0.01
- [ ]  $1

    


---
## Serum Basics

## What is Serum?
Serum is a highly efficient decentralized exchange built using the Solana network which makes it quite scalable and thus widely acceptable exchange. 
The prime differentiating factor between Serum and other popular DEXs is that it uses Central Limit Order Book (CLOB) rather than 
the Automated Market Makers (used by others) to enable effective and fast trading on its platform. 
The inbuilt design of Serum is empowered by its on-chain CLOB which is a largely used mechanism in traditional finance and 
further Serum makes it permissionless and decentralized, helping its users to erode a middle party and 
communicate directly with a smart contract to execute trades from an order book. Solana, which is one of the key components of the exchange, 
provides high speed and low cost to the Serum Network making it outshine other popular decentralized exchanges, enabling Serum to perform effectively. 
Moreover, Serum can interact efficiently with Ethereum and it enables high composability within the decentralized finance environment 
by permitting other smart contract protocols to interact with its order book to share liquidity and develop more features on top of it. 
Serum can be used alongside Ethereum.

## Serum's Central Limit Order Book
Serum has developed a fully decentralized, on-chain order book that allows users to execute orders directly with a smart contract. 
This system is still flexible with pricing and order sizes. Additionally, the CLOB and the matching engine provide both liquidity and 
price-time-priority-basis matching to users. As a result, users are able to choose the price, size, and 
direction of their trades without all of the associated inefficiencies. Moreover, Serum can be used for bootstrapping liquidity and matching services, 
adding an additional layer of composability to other protocols within the space.

Building on Solana allows Serum to use the network's high speed and low transaction costs to power its order book functionality and 
further reduce capital inefficiency and liquidity segmentation, while still being able to communicate with Ethereum. 
Serum's CLOB is designed to be able to handle any type of Solana-based trading instrument, such as options and futures, 
and is also comprised of other powerful features such as cross-chain swaps (e.g. ETH and BTC). 
Its architecture is designed in a way that makes it easier to add on or take away features as needed. 
This makes it a more flexible option for financial applications that need to share middleware.

By having an order book that is completely on the blockchain, Serum has made a lot of the benefits 
that come with traditional order books available to the DeFi ecosystem while still keeping the decentralized exchange effective. 
Additionally, this method allows other programs to connect to Serum and use its infrastructure to build their own project; 
for example, a trading application that would use the liquidity on Serum.

## Some useful links
- [Awesome Serum](https://github.com/project-serum/awesome-serum)
- [Solnet.Serum](https://github.com/bmresearch/Solnet.Serum) (.NET integration for Serum)
- [PySerum](https://github.com/serum-community/pyserum) (Python client library for interacting with Serum)
- [serum-ts](https://github.com/project-serum/serum-ts)
- [serum-dex](https://github.com/project-serum/serum-dex)


    


---
## Evaluation





##### Serum uses which of the following blockchains?  

- [ ]  Ethereum
- [ ]  Cardano
- [x]  Solana
- [ ]  Hyperledger





##### Central Limit Order Book functions well under which of the following scenarios?  

- [x]  Fast Transaction Speed
- [ ]  Slow Transaction Speed
- [ ]  High Gas Cost
- [x]  Low Gas Cost





##### What are the advantages of Central Limit Order Book over Automated Market Maker?  

- [x]  It works on First in First Out Principle (FIFO)
- [x]  It reduces liquidity segmentation
- [ ]  It increases liquidity segmentation
- [x]  It reduces capital inefficiencies

    


---
## Ideas for Serum based projects

The project ideas for Serum are numerous. Some of them, like the AMM Bot and cross-chain bridges, 
are about the technical aspects of the project. Other ideas, like Borrow/lending and Sushi Swap, 
are about applications that could be built on top of Serum. still others, like badges and GUI, 
are about ways to make the project more user-friendly.
[Here](https://docs.projectserum.com/serum-ecosystem/build-on-serum/project-ideas-for-serum#project-ideas) is a non-exhaustive list of projects people have asked for! 
Keep in mind there are loads of other great ideas out there too.

## Newest Project Requests
* Run Serum Markets on Testnet or Stress competition
  It would be valuable for people to run Serum markets on testnet and stress test to break them. 
  The following tasks need to be completed in order to do this: 
  - Manage DEX upgrades
  - List markets with tokens
  - Provide a faucet (for those tokens) that anyone can use to get and trade on the testnet DEX
  - Provide a GUI for the testnet DEX
  - Run cranks on the testnet DEX

* Introduce Streaming for Data from Solana Blockchain
  It would be beneficial to have the ability to change how Solana collects information from polling to streaming. 
  This would allow for the development of more sophisticated and hybrid apps on Solana. 
  The main issue with data feeds is that the only way to get information from the blockchain is to poll a validator / RPC node. 
  This is a problem for Serum because it makes it difficult to introspect the orderbook and to be notified as a maker when an order is matched. 
  Adding this functionality to the validator codebase would help solve this problem.
  The current design of a Solana validator uses rocksdb for quick storage of data and state, with periodic commits to a Google BigTable. 
  The proposal is to add code to the rocksdb interface that intercepts all database calls and streams that information out. 
  This would allow a secondary server to handle processing, filtering, analyzing, and distributing that information. 
  The implications of this would be very significant.


    


---
## Evaluation





##### Which of the following is used by current design of Solana validator for quick storage of data?  

- [ ]  Mongodb
- [x]  Rocksdb
- [ ]  MySql
- [ ]  Redis





##### Which of the following tasks are needed to be done so that people can run Serum markets on Testnet?  

- [x]  List markets with tokens
- [x]  Provide a faucet that anyone can use to get and trade on the testnet DEX
- [ ]  Remove rocksdb
- [x]  Provide a GUI for the testnet DEX

    


---
## What is a Good Project?

For your project to become huge or big at least, you need to keep a few things in mind so that your project actually adds value.
  - **Usefulness**: The project has to be useful. Think about what users want. 
    It's important to make sure that, if you build it, people will want to use it. 
    That's why most of the big projects in DeFi focus on areas like borrow/lending, DEXes, oracles, etc. 
    Even among the forks, the ones that have been successful are the ones that kept building and pushing the project in a new direction.
  - **Well-built**: Efficacy, intuitiveness, security, and reliability are some of the tell-tale traits of a good project. 
    You can always improve upon your work, no matter how good it may be, and there are always things you can do to make it just a little bit better. 
    Sometimes it's the small things that matter most. But in addition to generic improvements, there are also a bunch of specific ways to make your work stand out.
    * A really common mistake people make is forgetting that the average person doesn't know how to code. 
      Most people are more visual and want an interface that is easy to use and self-explanatory. 
      When designing your project's GUI, think about how someone with no coding experience would use it and 
      what would be the most intuitive way for them to get the information they need from it.
    * Before you launch a product, you need to make sure that it works well and doesn't have any bugs. 
      The "audit" process is often overemphasized; in reality, most projects have so many bugs that they need a thorough set of fixes before an audit can even begin. 
      And many projects that pass an audit still have fatal flaws. To test your product, use it yourself and look for common problems; 
      stress-test it; and show it to other people who you think will be able to give you honest feedback.
  - **Users**: The hardest part of making something great isn't the actual product creation. 
    It's getting users to adopt and use it. There are a few ways you can go about this:
    * If you have a large pre-existing user base, show the product to them first 
    * Hire a team for marketing and customer service 
    * Join forces with an existing project that has more reach 
    * In the extreme case, buy ads for it 
    Any of these can be successful in getting users, however, most projects fail because they don't do any of them.


    


---
## Evaluation





##### What are some of the tell-tale traits of a good project?  

- [x]  Efficacy
- [x]  Intuitiveness
- [x]  Security
- [ ]  None of the above





##### Which of the following is not a characteristic of a well-built project?  

- [x]  Unintuitive GUI
- [x]  Bugs in project launch
- [ ]  Performing stress-test before launch
- [ ]  High user adoption

    


---
## Grant

**EcoSerum**: EcoSerum is a community of active investors, traders, and developers who are committed to building the Serum ecosystem. 
Serum's success depends on factors like liquidity, accessibility, and community involvement. 
Stakers are SRM holders who are committed to Project Serum's long-term growth and supporting the development of user-friendly tools and applications.

**Grants**: Any project that is built using Serum can apply for a grant. This is a great way to get funding for your project, 
and it can help you to improve the quality of your project. This money can help cover expenses related to project development and implementation.
To apply for an EcoSerum grant, you'll need to prepare the following:
- A pitch deck outlining your product vision/roadmap, team, and demos (if any)
- Grant size request, and specific use of proceeds
- A thorough explanation of how your project will work with Serum (e.g. composition with Serum order book markets to match spot orders, liquidate positions, rebalance portfolios, etc.)

Once collected, you can e-mail the above information to grants@projectserum.com along with your request and any inquiries.

**Note**: Although there's no guarantee you'll get a grant, you can always reach out to grants@projectserum.com to see if your project idea is eligible. They may be able to help you out and provide some guidance.


    


---
## Evaluation





##### Select the correct statement(s) about grants?  

- [ ]  Only a project with very big user base can apply for grant
- [x]  Any project that is built using Serum can apply for a grant
- [x]  There is a guaranteed grant for every project
- [ ]  There is no guaranteed grant for any project





##### What is EcoSerum?  

- [ ]  A project built using Serum
- [ ]  An eco-friendly version of Serum
- [x]  A community of active investors, traders, and developers who are committed to building the Serum ecosystem
- [ ]  None of the above

    


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
    
