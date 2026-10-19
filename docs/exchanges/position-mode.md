# Position mode

Exchanges that support both long and short positions handle them in one of two
ways. Which mode your account uses determines which alert format you should
send.

## The two modes

**One-way mode** keeps a single net position per symbol. Buying while short
reduces the short. You cannot be long and short at the same time. This is the
default on most exchanges, and it is what
[standard alerts](../alerts/types/standard.md) expect.

**Hedge mode** keeps long and short positions separately. Both can be open on
the same symbol at once, each with its own size and entry price. This is what
makes it possible to run two strategies on one symbol without them closing
each other's trades.

## Matching your alerts to your account

| Your account is in | Send |
|---|---|
| One-way mode | [Standard alerts](../alerts/types/standard.md) |
| Hedge mode | [Hedge mode alerts](../alerts/types/hedge-mode.md) |

!!! warning "Mismatches are rejected"
    Sending standard alerts to a hedge-mode account, or hedge alerts to a
    one-way account, produces rejections from the exchange.

    If orders stopped placing right after you changed a setting on your
    exchange, this is the usual cause.

## Setting the mode

Position mode is set at your exchange, not in Tickerly. It is a trading
preference rather than an API permission, so you will not find it on the API
key screen.

=== "Bybit"

    Set per symbol. Open the symbol, select the three dots in the top right,
    choose **Trading Preferences**, then set **Position Mode**.

    Because it is per symbol, changing it for BTCUSDT does not change it for
    anything else.

=== "Binance"

    Set for all futures at once. Go to futures, select the three dots in the
    top right, choose **Preferences**, then set **Position Settings**.

=== "Bitget"

    Bitget defaults to hedge mode rather than one-way.

=== "Oanda"

    No account setting is needed. Standard alerts work either way.

=== "MetaTrader"

    Determined by the account your broker issued — netting or hedging. It
    cannot be changed from the terminal; your broker must supply the other
    account type.

## Changing mode on an open position

Exchanges generally refuse to switch position mode while a position is open or
an order is resting. Close everything on the account first, then change the
mode, then restart your strategies.

This also means a mode change is a good moment to check that your strategies
and your exchange agree on what is open. See
[Entries, exits and positions](../alerts/positions.md).

## Spot markets

Spot markets have no position mode. You hold an asset or you do not, and there
is nothing to short. Hedge mode does not apply to spot connections, and a
strategy that takes short positions cannot be automated against one.

## Next

- [Supported exchanges](supported.md)
