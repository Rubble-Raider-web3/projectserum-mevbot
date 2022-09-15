 ## Swap Program Basics

 One should have a basic understanding of the on-chain
 [Swap](https://github.com/project-serum/swap) program before using the
 client. Two core APIs are exposed.

 * [swap](https://github.com/project-serum/swap/blob/master/programs/swap/src/lib.rs#L36) -
   swaps two tokens on a single A/B market. This is just an IOC trade at the
   BBO that instantly settles.
 * [swapTransitive](https://github.com/project-serum/swap/blob/master/programs/swap/src/lib.rs#L107) -
   swaps two tokens across **two** A/x, B/x markets in the same manner as
   `swap`.

 When swapping to/from a USD(x) token, the swap client will use the `swap` API.
 When swapping to/from a non-USD(x) token, e.g., wBTC for wETH, the swap
 client will use the `swapTransitive`API with USD(x) quoted markets to bridge
 the two tokens.

 For both APIs, if the number of tokens received from the trade is less than
 the client provided `minExpectedAmount`, the transaction aborts.

 Note that if this client package is insufficient, one can always use the
  Anchor generated client directly, exposing an API mapping one-to-one to
 these program instructions. See the
 [`tests/`](https://github.com/project-serum/swap/blob/master/tests/swap.js)
 for examples of using the Anchor generated swap client.

 ## Serum Orderbook Program Basics

 Additionally, because the Swap program is an on-chain frontend for the Serum
 DEX, one should also be aware of the basic accounts needed for trading on
 the Serum DEX.

 Namely, a wallet must have an "open orders" account for each market the
 wallet trades on. The "open orders" account is akin to how a wallet
  must have an SPL token account to own tokens, except instead of holding
 tokens, the wallet can make trades on the orderbook.

 ### Creating Open Orders Accounts

 When the wallet doesn't have an open orders account already created,
 the swap client provides two choices.

 1. Explicitly open (and close) the open
    orders account explicitly via the [[initAccounts]]
    (and [[closeAccounts]]) methods.
 2. Automatically create the required accounts by preloading the instructions
    in the [[swap]] transaction.

 Note that if the user is swapping between two non-USD(x) tokens, e.g., wBTC
 for wETH, then the user needs *two* open orders accounts on both wBTC/USD(x)
 and wETH/USD(x) markets. So if one chooses option two **and** needs to
 create open orders accounts for both markets, then the transaction
 is broken up into two (and `Provider.sendAll` is used) to prevent hitting
 transaction size limits.

## Estimate and Swap

#### Estimate
Returns an estimate for the number of *to*, i.e., output, tokens one would
get for the given swap parameters. This is useful to inform the user
approximately what will happen if the user executes the swap trade. UIs
should use this in conjunction with some bound (e.g. 5%), to prevent users
from making unexpected trades.

```javascript
  // Estimate the amount received by swapping from SRM -> USDC.
  const estimatedUsdc = await client.estimate({
    fromMint: SRM,
    toMint: USDC,
    amount: toNative(1),
  });

```

#### Swap

Executes a swap against the Serum DEX on Solana. When using one should
first use `estimate` along with a user defined error tolerance to calculate
the `minExpectedSwapAmount`, which provides a lower bound for the number
of output tokens received when executing the swap. If, for example,
swapping on an illiquid market and the output tokens is less than
`minExpectedSwapAmount`, then the transaction will fail in an attempt to
prevent an undesireable outcome.

```javascript
  // Swaps SRM -> USDC on the Serum orderbook. If the resulting USDC is
  // has greater than a 1% error from the estimate, then fails.
  const usdcSwapTx = await client.swap({
    fromMint: SRM,
    toMint: USDC,
    amount: toNative(1),
    minExpectedSwapAmount: estimatedUsdc.mul(new BN(99)).div(new BN(100)),
  });
```

## Transitive Swap
Swaps two base currencies across two different markets.

That is, suppose there are two markets, A/USD(x) and B/USD(x).
Then swaps token A for token B via

* IOC (immediate or cancel) sell order on A/USD(x) market.
* Settle open orders to get USD(x).
* IOC buy order on B/USD(x) market to convert USD(x) to token B.
* Settle open orders to get token B.

Arguments:

* `amount`            - The amount to swap *from*.
* `min_exchange_rate` - The exchange rate to use when determining
whether the transaction should abort.

The only constraint imposed on these accounts is that the from market's
base currency's is not equal to the to market's base currency. All other
checks are done by the DEX on CPI (and the quote currency is ensured to be
the same on both markets since there's only one account field for it).

To perform a transitive swap, one sells on one market and buys on
another, where both markets are quoted in the same currency. Now suppose
one swaps A for B across A/USDC and B/USDC. Further suppose the first
leg swaps the entire *from* amount A for USDC, and then only half of
the USDC is used to swap for B on the second leg. How should we calculate
the exchange rate?

