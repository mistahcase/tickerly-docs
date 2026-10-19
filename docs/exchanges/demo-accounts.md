# Demo and test accounts

Most exchanges offer a demo, test, paper or sandbox account that trades with
simulated money. Tickerly can trade those accounts, which makes them the right
way to check a strategy end to end before risking anything.

## Enabling it

There is no separate setting. Tickerly routes a connection to your exchange's
demo environment when the connection's name contains the word **test**.

When adding the exchange, put `test` anywhere in **EXCHANGE ACCOUNT NAME**:

| Account name | Routes to |
|---|---|
| `Bybit test` | Demo |
| `test - Binance futures` | Demo |
| `Testing Oanda` | Demo |
| `Bybit live` | Live |
| `My Bybit account` | Live |

Capitalisation does not matter. Anything containing the letters `test` as a
word or inside a longer word is treated as a demo connection.

!!! warning "Choose the name carefully in both directions"
    A live connection accidentally named `Latest strategy` contains `test` and
    will be routed to the demo environment — your orders will never reach your
    live account.

    A demo connection without `test` in the name will be sent to your live
    account instead. Check the name before you enable a strategy.

## Use demo credentials

Demo environments issue their own API keys. A key from your live account will
not work against the demo environment, and vice versa.

Create the API key while logged into the demo account, then enter those
credentials on a connection whose name contains `test`.

## Running both at once

Live and demo connections are separate, each with its own pointer. A common
pattern is to keep both and switch a strategy between them by changing one
value in the alert message:

```json
"pointer" : "your-test-pointer"
```

Change it back to the live pointer when you are satisfied. Nothing else in the
alert needs to change.

## What demo accounts will not tell you

A demo account proves your alerts are well-formed, your credentials work, and
your strategy fires when you expect. It does not reproduce live trading
faithfully:

- Fills are often simulated at better prices than you would really get
- Slippage and partial fills may not occur at all
- Demo environments usually list fewer instruments than live accounts
- Some exchanges run demo environments on older software, so behaviour can
  differ from live

Treat demo trading as a test of the plumbing rather than a test of the
strategy's profitability.

## Supported exchanges

Most supported exchanges offer a demo environment, but not all, and some offer
one for certain markets only. If orders on a `test` connection are rejected
with credential or availability errors, check whether your exchange runs a
demo environment for that market.

## Next

- [Position mode](position-mode.md)
- [Supported exchanges](supported.md)
