# The connector

The Tickerly TV Connector is an Expert Advisor that runs in your MetaTrader
terminal. It collects orders from Tickerly and places them with your broker.

For the installation walkthrough, see
[Connect TradingView to MetaTrader](https://tickerly.net/connect-to-metatrader/).
This page is the reference for what it needs in order to work.

## Getting the connector

The simplest route is the MQL5 marketplace, from inside your terminal: search
for **Tickerly TV Connector** and install it there. The terminal handles
placement and updates for you.

If you install it manually instead, the file goes in the terminal's Experts
folder — reachable through **File → Open Data Folder** — and the terminal
needs a refresh of the Expert Advisors list before it appears.

## Three settings it depends on

All three are in the terminal, not in Tickerly. Miss any one and no orders
will place.

### 1. Algorithmic trading enabled

The **Algo Trading** button in the toolbar must be switched on. When it is
off, the connector runs but cannot place anything.

### 2. The Tickerly address allowed for web requests

Under **Tools → Options → Expert Advisors**, tick **Allow WebRequest for
listed URL** and add:

```
http://alerts.tickerly.net
```

!!! warning "Enter the address exactly"
    MetaTrader matches this entry strictly. `https://` instead of `http://`,
    a trailing slash, or a typo means the connector cannot reach Tickerly at
    all, and it fails quietly — the terminal shows no error, orders simply
    never arrive.

    If the connector appears to be running but nothing ever trades, check
    this first.

### 3. The connector attached to a chart

Drag the connector onto any chart, or right-click it and choose **Attach to
chart**. The symbol and timeframe of that chart are irrelevant — the
connector trades whatever your alerts specify, not what the chart shows.

One connector per terminal is enough. It handles every symbol.

## Settings

The connector needs your **pointer**, taken from the Tickerly connection you
created. Enter it in the connector's inputs when attaching it to the chart.

The pointer is what tells Tickerly which terminal an order belongs to. If you
run more than one terminal, each needs its own connection and its own
pointer — the same pointer in two terminals means orders are collected by
whichever one asks first.

## Checking it is working

A smiling face in the top right corner of the chart means the connector is
attached and algorithmic trading is on. A sad face means it is attached but
not permitted to trade.

For an end-to-end test, send a
[manual alert](https://tickerly.net/trigger-manual-alert/) from Tickerly and
watch for the order in your terminal. That tests the whole path — queue,
connector, broker — without waiting for your strategy to fire.

## When orders stop arriving

In rough order of likelihood:

- Algorithmic trading was switched off, often by a terminal restart
- The terminal was closed, slept, or lost its internet connection
- The WebRequest address was removed, or a terminal update reset the list
- The connector was removed from the chart, or the chart was closed
- The wrong pointer is in the connector's settings
- The terminal is running but the broker's connection has dropped — check the
  connection status in the bottom right corner

Because queued orders are discarded after 60 seconds, an interruption of any
length means those signals are gone rather than delayed. After restoring a
terminal, check whether your strategy and your broker still agree on what is
open — see
[Entries, exits and positions](../alerts/positions.md).

## Next

- [Differences from direct connections](differences.md)
