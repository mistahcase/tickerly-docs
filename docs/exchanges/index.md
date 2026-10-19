# Exchange connections

An exchange connection links your Tickerly account to one trading account at
one exchange. It holds the API credentials Tickerly uses to place orders, and
it has a **pointer** — the value your alerts carry to say where an order
should go.

Setting one up has two parts:

1. **At your exchange** — create an API key with the right permissions.
2. **At Tickerly** — enter those credentials under
   [Add exchange](https://tickerly.net/add-formidable-exchange/).

The step-by-step walkthrough lives on the main site:
[Exchange setup](https://tickerly.net/exchange-setup/). This section covers
what each field means and what each exchange requires.

## One connection, any number of strategies

A connection is not tied to a strategy. Once it exists, every alert carrying
its pointer is routed to that trading account, whatever strategy sent it. You
never configure a strategy on the Tickerly side.

Create more than one connection when you want to:

- Trade on more than one exchange
- Separate live and demo trading — see [Demo and test accounts](demo-accounts.md)
- Keep sub-accounts apart, so their positions and balances stay independent
- Use different market types on the same exchange, such as Binance spot and
  Binance USD-M futures, which are separate connections

## Pointers

Each connection gets its own pointer when you create it. Find it, along with a
ready-made alert message, under
[My exchanges](https://tickerly.net/view-my-exchanges/).

The pointer is the one part of the standard alert message you must fill in
yourself. Copying the ready-made message from My exchanges does this for you.

!!! note "Treat your pointer as private"
    A pointer identifies your account and directs orders to it. It carries no
    credentials and cannot be used to withdraw funds or read your API keys,
    but anyone who has it can send alerts that place orders on your account.
    Avoid posting alert messages publicly with the pointer left in.

## What Tickerly does with your credentials

Your API keys stay in your exchange connection. They are never part of an
alert, which is why alert messages contain nothing sensitive beyond the
pointer.

Orders reach your exchange from a single address:

```
217.160.238.76
```

Whitelist it where your exchange supports API whitelisting — see
[API keys and permissions](api-keys.md).

## In this section

- [API keys and permissions](api-keys.md) — what to enable, what to leave off
- [Credential fields by exchange](credentials.md) — which value goes in which box
- [Demo and test accounts](demo-accounts.md)
- [Position mode](position-mode.md) — one-way and hedge
- [Supported exchanges](supported.md) — the full list and what each supports
