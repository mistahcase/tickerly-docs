# Full trade alerts

An entry order that carries its own take profit and stop loss levels, so the
exchange manages the exit.

## Purpose

With a standard alert, a position closes when a second alert arrives. With a
full trade alert, the exit levels are attached to the entry and held by the
exchange, so the position closes whether or not another alert ever arrives.

Tickerly is alerted on entry only. Do not send exit alerts for a strategy
using this format.

## Message

```json
{
"ticker": "{{ticker}}",
"action": "{{strategy.order.action}}",
"prev_position" : "{{strategy.prev_market_position}}",
"quantity": "{{strategy.order.contracts}}",
"takeprofit" : "{{plot_0}}",
"stoploss" : "{{plot_1}}",
"pointer" : "replace with pointer for your exchange here"
}
```

This assumes your strategy plots its take profit level as `plot_0` and its
stop loss as `plot_1`. Change the numbers to match your strategy.

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `ticker` | Yes | |
| `action` | Yes | `buy` or `sell` |
| `prev_position` | Yes | |
| `quantity` | Yes | |
| `takeprofit` | Yes | An absolute price |
| `stoploss` | Yes | An absolute price |
| `pointer` | Yes | |
| `stoplosstype` | Optional | Capital.com only — `normal` |

Take profit and stop loss are **absolute price levels**, such as `43324`, not
distances, percentages or pip counts. Both must be sent together.

## Behaviour

The levels are passed to your exchange with the entry order. From that point
the exchange owns the exit: it closes the position when price reaches either
level, with no further involvement from Tickerly or TradingView.

Your alert log records the entry. Exits triggered by these levels happen at
the exchange and appear in your exchange's own order history.

## Setting levels inside the strategy

If your strategy has input boxes for custom messages on each action, you can
put the alert messages there instead, using whatever placeholders your
strategy exposes for its calculated levels.

```json
{
"ticker": "#symbol#",
"action": "buy",
"prev_position" : "flat",
"quantity": "#quantity#",
"takeprofit" : "#longTpPrice#",
"stoploss" : "#longStopPrice#",
"pointer" : "replace with pointer for your exchange here"
}
```

```json
{
"ticker": "#symbol#",
"action": "sell",
"prev_position" : "flat",
"quantity": "#quantity#",
"takeprofit" : "#shortTpPrice#",
"stoploss" : "#shortStopPrice#",
"pointer" : "replace with pointer for your exchange here"
}
```

Placeholder names differ between strategies — check yours. Any of them can be
replaced with a fixed value.

When the messages live inside the strategy, the alert message box itself
usually contains only:

```
{{strategy.order.alert_message}}
```

Some strategies use `{{strategy.order.comment}}` instead. Test which one your
strategy fills in.

## Capital.com stop loss types

Capital.com defaults to a **guaranteed** stop loss, because some countries
require one. If normal stops are permitted where you trade, add:

```json
"stoplosstype" : "normal"
```

Guaranteed stops have their own minimum distance requirements. A stop placed
too close to the current price is rejected by Capital.com.

## Exchange support

Bybit, Binance, Oanda and Capital.com.

On other exchanges, use a standard alert and let your strategy send its own
exit alerts.

## Constraints

**Both levels are required.** Send `takeprofit` and `stoploss` together.

**Your strategy must expose the levels.** Not every strategy plots its exit
levels or offers them as placeholders. If yours does not, use a
[standard alert](standard.md).

**Levels must be valid for the instrument.** Exchanges enforce minimum
distances from the current price and maximum distances overall. Levels outside
those bounds are rejected.

## See also

- [Field reference](../fields.md)
- [Choosing an alert type](../choosing.md)
