

## General Working of a Liquidity Pool
The Pool account is designed to hold assets. You can create a Pool account by sending in some assets. In return, you will 
be given a Pool Token (T) which represents your partial ownership of the Pool.

If you send in x% of the Pool's assets, the Pool will mint enough T tokens so that you have x% of the prior outstanding. 
You can redeem your T tokens to get back assets. However, if the Pool has changed since you created it, you 
might not get back exactly the same thing.

If you have x% of all Pool Tokens, you are entitled to x% of the assets in the Pool (see below). When a Token is redeemed, 
it is burned.

You can customize if and how the Pool can trade or rebalance its assets.

This is what represents a pool state in Serum
```rust

pub struct PoolState {
    pub tag: PoolStateTag,

    /// Token mint account for the pool token.
    pub pool_token_mint: Address,
    /// Mint and vaults for the assets in the pool.
    pub assets: Vec<AssetInfo>,

    /// Mint authority for the pool token and owner for the assets in the pool.
    pub vault_signer: Address,
    /// Nonce used to generate `vault_signer`.
    pub vault_signer_nonce: u8,

    /// Additional accounts that need to be included with every request.
    pub account_params: Vec<ParamDesc>,

    /// User-friendly pool name.
    pub name: String,

    /// Vault for fees collected by the pool for LQD. Mint is the pool token mint.
    pub lqd_fee_vault: Address,
    /// Vault for fees collected by the pool for the pool initializer. Mint is the pool token mint.
    pub initializer_fee_vault: Address,

    /// Fee on creations and redemptions, per million tokens.
    pub fee_rate: u32,

    /// Meaning depends on the pool implementation.
    pub admin_key: Option<Address>,

    pub custom_state: Vec<u8>,
}

pub struct AssetInfo {
    pub mint: Address,
    /// Vault should be owned by `PoolState::vault_signer`
    pub vault_address: Address,
}

```
## Pool Operations - Create pool tokens
When initializing the pool you need to  specify the creation basket for Pool Tokens. It takes N, the number of Pool Tokens you want to 
create, as an argument. It outputs the assets and quantities needed to create the N number of T tokens. You can also enter a fixed creation 
basket per T if you want. If you leave this blank, it defaults to `N / (total number of T)` * (full contents of basket). 
Note that the function can also return assets to the pool before returning the basket.

For creations, the basket is the quantity of each asset that need to be sent to the pool to process the creation. For
redemptions and swaps, the basket is the quantity of each asset that will be transferred from the pool to the user after
the redemption or swap.

When you create N number of tokens of type T:
* P takes the creation basket for N, and mints you N of T
* If you don’t have the creation basket it fails

```js
import { Account, Connection, PublicKey } from '@solana/web3.js';
import { loadPoolInfo, PoolTransactions } from '@project-serum/pool';
import BN from 'bn.js';

let connection = new Connection('...');
let poolAddress = new PublicKey('...'); // Address of the pool.
let payer = new Account('...'); // Account to pay for solana fees.

let poolInfo = await loadPoolInfo(connection, poolAddress);
let { transaction, signers } = PoolTransactions.execute(
  poolInfo,
  {
    // Number of tokens to create.
    create: new BN(100),
  },
  {
    // Spl-token account to send the created tokens.
    poolTokenAccount: new PublicKey('...'),
    // Spl-token accounts to pull funds from.
    assetAccounts: [new PublicKey('...'), new Public('...')],
    // Owner of poolTokenAccount and assetAccounts.
    owner: payer.publicKey,
  },
  // Expected creation cost.
  [new BN(10), new BN(10)],
);
await connection.sendTransaction(transaction, [payer, ...signers]);
```

## Pool Operation - Get pool basket
Use [`getPoolBasket()`](https://project-serum.github.io/serum-ts/pool/modules/_index_.html#getpoolbasket)
to fetch the current pool basket (the quantity of each token needed to create N pool tokens
or the quantity of each token received for redeeming N pool tokens).

Negative quantities will cause tokens to be transferred in the opposite direction.

```js
import { Connection, PublicKey } from '@solana/web3.js';
import { loadPoolInfo, getPoolBasket } from '@project-serum/pool';
import BN from 'bn.js';

let connection = new Connection('...');
let poolAddress = new PublicKey('...'); // Address of the pool.

let poolInfo = await loadPoolInfo(connection, poolAddress);
let basket = await getPoolBasket(
  connection,
  poolInfo,
  { create: new BN(100) },
  // Arbitrary SOL address, can be anything as long as it has nonzero SOL
  // and is not a program-owned address.
  new PublicKey('...'),
);

console.log(basket);
```

## Pool Operations - Redeem pool tokens
When you redeem N of T:
* Pool burns N of T
* Pool returns the redemption basket for N of T to you

Send a transaction to redeem pool tokens:
```js
import { Account, Connection, PublicKey } from '@solana/web3.js';
import { loadPoolInfo, PoolTransactions } from '@project-serum/pool';
import BN from 'bn.js';

let connection = new Connection('...');
let poolAddress = new PublicKey('...'); // Address of the pool.
let payer = new Account('...'); // Account to pay for solana fees.

let poolInfo = await loadPoolInfo(connection, poolAddress);
let { transaction, signers } = PoolTransactions.execute(
  poolInfo,
  {
    // Number of tokens to redeem.
    redeem: new BN(100),
  },
  {
    // Spl-token account to pull the pool tokens to redeem from.
    poolTokenAccount: new PublicKey('...'),
    // Spl-token accounts to send the redemption proceeds.
    assetAccounts: [new PublicKey('...'), new Public('...')],
    // Owner of poolTokenAccount and assetAccounts.
    owner: payer.publicKey,
  },
  // Expected redemption proceeds.
  [new BN(10), new BN(10)],
);
await connection.sendTransaction(transaction, [payer, ...signers]);
```
