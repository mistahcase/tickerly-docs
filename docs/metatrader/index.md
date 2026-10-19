# MetaTrader connections

MetaTrader connections work differently from every other exchange in
Tickerly. There are no API keys. Instead, a small program called the
**connector** runs inside your own MetaTrader terminal, collects orders from
Tickerly, and places them through the terminal.

That makes MetaTrader the broadest option Tickerly offers — it works with any
broker or prop firm that supports MT4 or MT5, across any market they
offer — and it also means your terminal has to be running for anything to
trade.

The step-by-step walkthrough is on the main site:
[Connect TradingView to MetaTrader](https://tickerly.net/connect-to-metatrader/).
This section covers what the connection requires and how its behaviour
differs from a direct exchange connection.

## How an order reaches your broker

1. Your strategy fires an alert, which goes to Tickerly as normal.
2. Tickerly validates it and places the order in a queue for your pointer.
3. The connector in your terminal collects the order and places it with your
   broker.
4. The result is reported back and appears in your alert log.

The difference from a direct connection is step 3. Tickerly does not reach
into your terminal — the connector comes and gets the order.

!!! warning "Queued orders expire after 60 seconds"
    If the connector does not collect an order within 60 seconds, the order is
    discarded and you are notified. It is not placed late.

    A terminal that is closed, asleep, offline, or has algorithmic trading
    switched off will miss every order sent to it.

## What you need

| | |
|---|---|
| Plan | Level 4 or above |
| Terminal | MetaTrader 4 or MetaTrader 5 |
| Account | Any broker or prop firm offering MT4 or MT5 |
| Connector | Tickerly TV Connector, installed in the terminal |
| Credentials | None — the pointer identifies the connection |

Level 4 supports up to 10 concurrent terminals, each as its own connection
with its own pointer.

## Setting one up

Creating the connection itself takes three steps on the Tickerly side:

1. Under [Add exchange](https://tickerly.net/add-formidable-exchange/), choose
   **MetaTrader 5** or **MetaTrader 4** to match your terminal. The underlying
   broker does not matter.
2. Give it a name. If you run several terminals, name them so you can tell
   them apart later.
3. Note the pointer. It goes into the connector's settings, and into your
   alert messages.

No API credentials are entered, because none exist. The pointer is what links
your alerts, Tickerly's queue, and your terminal together.

Everything else happens in the terminal — see
[The connector](connector.md).

## Keeping it running

A direct exchange connection trades whether your computer is on or not. A
MetaTrader connection does not: if the terminal stops, trading stops, and any
orders sent meanwhile expire unfilled.

For anything beyond testing, run the terminal on a machine that stays on. A
VPS is the usual answer, and many brokers offer one free above a certain
account size or trading volume. A home machine works if you can guarantee it
will not sleep, restart for updates, or lose its connection.

## In this section

- [The connector](connector.md) — installation requirements and settings
- [Differences from direct connections](differences.md) — what changes about
  alerts, sizing and supported features
