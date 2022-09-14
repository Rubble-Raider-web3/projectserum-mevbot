## Accounts

#### Market
This will contain information about the different order books that are involved.

### Ask
This contains all the order submitted by the seller

### Bid
This contains all the order submitted by the buyer

### Request Queue

### Event Queue
Contains the events that are pushed on to it when trades happen.

### Order Payer Token Account
The account that funds will be taken from, i.e., transferred from the user into the market's vault.
For bids, this is the base currency. For asks, the quote.

### Coin Vault (Base Account)
Also known as the "base" currency and holds base currency balances. For a given A/B market, this is the vault for the A 
mint

### pc_vault (Quote Account)
Also known as the "quote" currency and holds quote currency balances. For a given A/B market, this is the vault for the 
B mint.



### Note - Base vs Quote
Like in a BTC/USDC market where generally the convention is you buy btc with USDC, so USDC would be on the bids
and BTC would be on the asks. Here the "quote" mint would be USDC and the base would be BTC

### vault_signer
PDA owner of the DEX's token accounts for base + quote currencies.

### coin_wallet
User wallets.

### How Trade happens on Serum
Every user on Serum has a special account called an open orders account. This account stores the state of all the assets 
the user has available to them. When an order is placed on the book, some of the tokens are locked up. This locked token 
quantity will be incremented every time a new order is placed. For example, if a user has some amount of USDC and they 
want to post a bid on the BTC-USTC order book, they are saying that the token they placed as a bid is no longer withdrawable 
from their account.

The locked quantity should be freed up in some way when the trade is completed. This means that the other token that was 
filled should be available for withdrawal.

Whenever you place a new order in the book, you'll need to transfer some tokens into Serum. Serum will take note of the 
amount of tokens you've transferred, and if you make a trade, what will generally happen is that some of the tokens will 
be transferred from the locked state to the freed state. You can always retrieve your free tokens if you need to. There 
will be a quantity of locked tokens and a quantity of freed tokens, and these will usually be prefaced with either 
"base" or "quote" based on the two types.

When you trade, you can think of these values as incrementing or decrementing based on the positions that occur. So, if 
you place an order in the book, your locked quantity will increase. What happens is that once that order gets processed 
in the event queue, this free quantity is going to increase. So, you can withdraw that.

There's an instruction that's
    ```rust
        pub struct NewOrderInstructionV1 {
            pub side: Side, # Ask or Bid
            pub limit_price: NonZeroU64, 
            pub max_qty: NonZeroU64, 
            pub order_type: OrderType, # Limit, ImmediateOrCancel, PostOnly
            pub client_id: u64,
        }
    ```
This instruction will check the existing orders to see if yours can be matched. It will take into account whether your 
order is a bid or an ask, and compare it to the other orders to see if there is a possible match. If there is, the two 
orders will be matched.

And in case the order don't match, it will the take order and put it in the order book tree structure.

## When Trade happens

When trade happens a fill event is pushed on the event queue
    ```rust
        Fill {
            side: Side,
            maker: bool,
            native_qty_paid: u64,
            native_qty_received: u64,
            native_fee_or_rebate: u64,
            order_id: u128,
            owner: [u64; 4],
            owner_slot: u8,
            fee_tier: FeeTier,
            client_order_id: Option<NonZeroU64>,
        },
    ```

## Cancel an order
cancel order instruction takes in the order id of the order that needs to be cancelled.


## Event Queue
At a very high level, when you process trades in the order book, there are two things you want to do. The first is to 
add the fill. So, if I have an order of size 10 and the trade size is also 10, then the order gets fully filled and two events 
are pushed into the queue: 
1) one for the fill 
2) one for the out

In this case, you would need both a fill event and an out event. The reason for this is because the user who placed the 
initial order that got filled (the passive fill) still has their order ID stored somewhere. Every time you place an order 
or make a trade, you need to copy that order ID key and put it in a static object that you store. When a trade gets filled, 
that particular party's order ID is still in their user account, so you need to remove it. That's why you need to push 
both a fill event and an out event.

In the case when order is only partially filled, there is only one event pushed and i.e. is partial fill. No out event is
pushed onto event queue in this case.







------


