# Supported exchanges

Each row is a connection type you can select when adding an exchange. Where an
exchange appears more than once, the entries are separate connections with
separate credentials, pointers and capabilities.

| Connection | Markets |
|---|---|
| Oanda | Forex, indices, commodities, metals, bonds |
| Capital.com | Forex, indices, commodities, shares, crypto |
| Alpaca | Stocks, indices, crypto |
| MetaTrader 4 | Any market your broker offers |
| MetaTrader 5 | Any market your broker offers |
| Binance | Crypto spot |
| Binance USD-M | Crypto futures, USD-margined |
| Binance COIN-M | Crypto futures, coin-margined |
| Binance US | Crypto spot, available to US residents |
| Bybit | Crypto futures |
| Bitget | Crypto futures |
| Kraken | Crypto spot |
| Kraken Margin | Crypto margin |
| Kraken Futures | Crypto futures |
| KuCoin | Crypto spot |
| KuCoin Futures | Crypto futures |
| Hyperliquid Spot | Crypto spot, decentralised |
| Hyperliquid Perps | Crypto perpetuals, decentralised |

## Feature support

| Connection | Hedge mode | Full trade | Close all | Demo account |
|---|:--:|:--:|:--:|:--:|
| Oanda | ● | ● | ● | ● |
| Capital.com | — | ● | ● | ● |
| Alpaca | — | — | — | ● |
| MetaTrader 4 | ● | — | — | ● |
| MetaTrader 5 | ● | — | — | ● |
| Binance | — | ● | ● | ● |
| Binance USD-M | ● | ● | ● | ● |
| Binance COIN-M | ● | ● | ● | ● |
| Binance US | — | ● | ● | — |
| Bybit | ● | ● | ● | ● |
| Bitget | ● | — | ● | ● |
| Kraken | — | — | ● | — |
| Kraken Margin | — | — | ● | — |
| Kraken Futures | — | — | ● | ● |
| KuCoin | — | — | ● | ● |
| KuCoin Futures | — | — | ● | ● |
| Hyperliquid Spot | — | — | ● | — |
| Hyperliquid Perps | — | — | ● | — |

● supported — not supported

[Standard](../alerts/types/standard.md), [swing](../alerts/types/swing.md) and
[directional](../alerts/types/directional.md) alerts work on every connection
and are not listed.

**Hedge mode** — running long and short on one symbol at once. See
[Hedge mode alerts](../alerts/types/hedge-mode.md).

**Full trade** — attaching take profit and stop loss to the entry order. See
[Full trade alerts](../alerts/types/full-trade.md).

**Close all** — flattening a symbol without stating a size. See
[Close all alerts](../alerts/types/close-all.md).

## Availability varies by country

Support here means Tickerly can place the order. Whether you can trade a given
market is decided by your exchange and your jurisdiction.

Exchanges routinely offer different asset classes in different countries.
Binance futures are unavailable in several jurisdictions; some instruments on
Capital.com and Oanda are restricted by region; Alpaca and Binance US serve
specific markets only.

!!! tip "Check availability at the exchange, not here"
    Before assuming an instrument is unsupported, confirm it is tradable on
    your own exchange account. Region, account type, verification level and
    demo-versus-live all affect what you can trade.

## Choosing a connection

Three things decide which you want:

**The market.** Spot and futures are separate connections on Binance, Kraken,
KuCoin and Hyperliquid. Pick the one matching what your strategy trades — a
strategy that goes short needs a futures, margin or CFD connection, since spot
cannot short.

**The features.** If your strategy needs hedge mode or full trade alerts, the
matrix above narrows the choice.

**Your broker, for MetaTrader.** MetaTrader connections work with any broker
or prop firm offering MT4 or MT5, so what you can trade is set by the broker
rather than by Tickerly. They behave differently from direct connections in
several ways — see [MetaTrader connections](../metatrader/index.md).

## Next

- [API keys and permissions](api-keys.md)
- [Choosing an alert type](../alerts/choosing.md)
