# Differences from direct connections

Most of Tickerly works identically on MetaTrader. This page covers the places
it does not, so you can tell a real problem from expected behaviour.

## Summary

| | MetaTrader | Direct exchange connections |
|---|---|---|
| Credentials | None — pointer only | API key and secret |
| Order delivery | Collected by your terminal | Sent straight to the exchange |
| If the link is down | Order expires after 60 seconds | Order is placed as normal |
| Swing strategies | Standard alert handles them | Needs a [swing alert](../alerts/types/swing.md) |
| Hedge mode | Set by your broker's account type | Set in your exchange account |
| [Full trade alerts](../alerts/types/full-trade.md) | Not supported | Bybit, Binance, Oanda, Capital.com |
| [Close all alerts](../alerts/types/close-all.md) | Not supported | Supported on most |

## Swing strategies need no special alert

On direct connections, a strategy that flips straight from long to short
needs a [swing alert](../alerts/types/swing.md), because ordinary closing
orders are reduce-only and cannot cross through zero.

On MetaTrader the standard alert already handles this. Use the standard
format for every strategy, including ones that reverse directly.

This is the main place where advice written for direct connections does not
apply to MetaTrader.

## No close all, no full trade

Close all alerts are not supported. To flatten a position, send an ordinary
exit alert stating the size, or close it in the terminal.

Full trade alerts are not supported either. Take profit and stop loss levels
must come from your strategy as separate exit alerts, or be managed in the
terminal.

## Hedge mode comes from your broker

On an exchange, position mode is a setting you change. On MetaTrader it is a
property of the account your broker issued:

- A **hedging** account can hold long and short positions on the same symbol
- A **netting** account keeps one net position per symbol

You cannot switch between them in the terminal. If you need hedging and have
a netting account, your broker has to supply a different account.

See [Hedge mode alerts](../alerts/types/hedge-mode.md).

## Order sizes are in units, not lots

Quantity is expressed the way TradingView expresses it, in units of the base
currency, not in lots.

| Alert sends | Equals |
|---|---|
| `1000` | 0.01 lots |
| `10000` | 0.1 lots |
| `100000` | 1 standard lot |

A strategy sized in lots needs converting; one sending
`{{strategy.order.contracts}}` from a chart already denominated in units
needs nothing.

!!! warning "Check the first live trade"
    A factor-of-100 sizing error is the most expensive mistake available here,
    and it looks like a working setup right up until the position appears.
    Place one trade on a demo account and confirm the lot size in the terminal
    before going live.

## Symbol names pass through as written

Tickerly converts ticker names into the format each exchange expects. For
MetaTrader it passes the symbol through as written, because brokers name
instruments inconsistently — `EURUSD`, `EURUSD.pro`, `EURUSD.raw` and
`EURUSDm` are all the same pair at different brokers.

If your broker uses a suffix, set the ticker explicitly in the alert to match
your terminal's Market Watch:

```json
"ticker": "EURUSD.pro"
```

See [Ticker formats](../alerts/tickers.md).

## Orders are queued, not placed instantly

On a direct connection, Tickerly sends the order to the exchange. On
MetaTrader, the order waits for the connector to collect it, which is normally
near-instant but depends on your terminal being reachable.

Your alert log reflects this: an order accepted by Tickerly and waiting shows
as queued rather than placed. A queued order that is never collected within
60 seconds is discarded, and you are notified.

In practice this means a MetaTrader setup needs the terminal watched in a way
a direct connection does not. See
[The connector](connector.md) for what to check when orders stop arriving.

## See also

- [MetaTrader connections](index.md)
- [Supported exchanges](../exchanges/supported.md)
