# Tickerly documentation

This is the technical reference for Tickerly. It describes exactly what each
part of an alert does, what values are accepted, and how Tickerly turns an
alert from TradingView into an order on your exchange.

If you are setting Tickerly up for the first time, start with the step-by-step
guides on the main site instead — they walk you through the process in order:

- [How automated trading works](https://tickerly.net/how-trading-automated-trading-works/)
- [Exchange setup](https://tickerly.net/exchange-setup/)
- [MetaTrader setup](https://tickerly.net/connect-to-metatrader/)
- [Setting up your trading bot](https://tickerly.net/set-alerts/)

Come back here when you need the precise answer: which fields are required,
what a particular value does, which alert type fits your strategy, or why an
alert behaved differently from how you expected.

## Where to start

| If you want to | Go to |
|---|---|
| Connect an exchange | [Exchange connections](exchanges/index.md) |
| Find which API credential goes in which field | [Credential fields](exchanges/credentials.md) |
| Check what your exchange supports | [Supported exchanges](exchanges/supported.md) |
| Set up MetaTrader | [MetaTrader connections](metatrader/index.md) |
| Understand the alert message | [Overview](alerts/index.md) |
| Look up a specific field | [Field reference](alerts/fields.md) |
| Know when Tickerly opens vs closes a position | [Entries, exits and positions](alerts/positions.md) |
| Work out which alert type you need | [Choosing an alert type](alerts/choosing.md) |

## Conventions

**Required** means the alert is rejected without it. **Conditional** means it is
required for some alert types and not others; the field's entry says which.
**Optional** means Tickerly works with or without it.

Field names and values are shown in `code font`. Field names and values are
written exactly as they must appear in your alert.

Exchange names refer to the connection type you selected when you added the
exchange in Tickerly — for example, *Binance USD-M* and *Binance US* are
separate connections with separate capabilities.
