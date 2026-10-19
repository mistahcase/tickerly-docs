# Choosing an alert type

Tickerly accepts six alert formats. Most strategies work with the standard
one. The others exist for specific situations.

## Start here

| If your strategy… | Use |
|---|---|
| Opens and closes positions normally, one at a time | [Standard](types/standard.md) |
| Flips straight from long to short without going flat | [Swing](types/swing.md) |
| Runs alongside another strategy on the same symbol | [Hedge mode](types/hedge-mode.md) |
| Sets its own take profit and stop loss levels at entry | [Full trade](types/full-trade.md) |
| Is an indicator, or needs a separate alert per direction | [Directional](types/directional.md) |
| Needs to close everything on a symbol regardless of size | [Close all](types/close-all.md) |

When in doubt, use the standard alert. It handles multiple take profits,
moving a stop loss to break even, dynamic order sizes, trailing stops, and
most complex entry and exit logic without any change.

## Field requirements at a glance

| | `ticker` | `action` | `prev_position` | `quantity` | `pointer` | Other |
|---|---|---|---|---|---|---|
| Standard | ● | ● | ● | ● | ● | |
| Swing | ● | ● | — | ● | ● | |
| Hedge mode | ● | ● | ● | ● | ● | `hedgemode` |
| Full trade | ● | ● | ● | ● | ● | `takeprofit`, `stoploss` |
| Directional | ● | ● | ● | ● | ● | |
| Close all | ● | — | — | — | ● | `closeall` |

● required — omitted

## How they differ

**Standard** is the default. One alert per strategy action, with
`prev_position` telling Tickerly whether to open or close.

**Swing** is the standard alert with `prev_position` removed. That makes every
order an entry, which lets a single order flip a position from long to short.
Use it only when your strategy actually does that — on a strategy that goes
through flat, it removes a useful safety property.

**Hedge mode** adds `hedgemode` so long and short positions on the same symbol
can exist side by side. It is the only way to run two strategies on one symbol
without them closing each other's trades.

**Full trade** carries take profit and stop loss prices with the entry, so the
exchange manages the exit. Tickerly is alerted on entry only. Use it when your
strategy plots its exit levels and you would rather the exchange enforce them
than depend on a second alert arriving.

**Directional** replaces one alert with four — enter long, exit long, enter
short, exit short — each with fixed values. It is how indicators are automated,
since indicators do not supply strategy placeholders.

**Close all** is not a strategy format but a utility. It closes whatever is
open on a symbol without needing to know the size.

## Combinations

Swing and hedge mode cannot be combined. Hedge mode handling requires
`prev_position`, and a swing alert is defined by leaving it out.

!!! tip "MetaTrader does not need swing alerts"
    On MetaTrader 4 and 5, the standard alert already handles strategies that
    flip directly between long and short. Use the standard format there.

Full trade and hedge mode can be combined where the exchange supports both.
Close all can be sent to any supported exchange at any time, whatever format
the strategy's own alerts use.
