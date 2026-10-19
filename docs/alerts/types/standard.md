# Standard alerts

The default alert format. Use it unless your strategy needs one of the
[other types](../choosing.md).

## Purpose

Replicates your strategy's orders exactly. One alert covers every action the
strategy takes — entries and exits, long and short — and Tickerly works out
which is which from the values in the message.

## Message

```json
{
"ticker": "{{ticker}}",
"action": "{{strategy.order.action}}",
"prev_position" : "{{strategy.prev_market_position}}",
"quantity": "{{strategy.order.contracts}}",
"pointer" : "replace with pointer for your exchange here"
}
```

Copy the ready-made version from
[My exchanges](https://tickerly.net/view-my-exchanges/), where the pointer is
already filled in.

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `ticker` | Yes | Converted to your exchange's format automatically |
| `action` | Yes | `buy` or `sell` |
| `prev_position` | Yes | `flat`, `long` or `short` |
| `quantity` | Yes | A number, in the instrument's own units |
| `pointer` | Yes | The only field you must change |

## Behaviour

`action` and `prev_position` together determine whether the order opens, adds
to, or closes a position. Closing orders are sent reduce-only, so they cannot
flip your position into the opposite direction. See
[Entries, exits and positions](../positions.md) for the full table.

No configuration is needed on the Tickerly side. Any number of strategies can
send standard alerts through the same pointer.

## What it supports

The standard format handles most strategy logic without modification,
including multiple take profits, moving a stop loss to break even, variable
order sizes, trailing stops, and pyramiding entries.

## Exchange support

All supported exchanges.

## Constraints

Because closing orders are reduce-only, a strategy that flips directly from
long to short in a single action cannot be automated with this format. Use a
[swing alert](swing.md) instead — except on MetaTrader 4 and 5, where the
standard format already handles it.

## See also

- [Setting up your trading bot](https://tickerly.net/set-alerts/) — step-by-step guide
- [Field reference](../fields.md)
