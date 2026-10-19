# Swing alerts

For strategies that move directly between long and short without going flat
in between.

## Purpose

A standard alert marks closing orders reduce-only, which stops them opening a
position in the opposite direction. That is usually a safeguard, but it blocks
strategies that are designed to flip in one action.

A swing alert removes `prev_position`, so every order is treated as an entry
and a single order can carry a position through zero — from long straight to
short, or the reverse.

## Message

```json
{
"ticker" : "{{ticker}}",
"action" : "{{strategy.order.action}}",
"quantity" : "{{strategy.order.contracts}}",
"pointer" : "replace with your pointer here"
}
```

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `ticker` | Yes | |
| `action` | Yes | `buy` or `sell` |
| `quantity` | Yes | Must cover both closing the old position and opening the new one |
| `pointer` | Yes | |
| `prev_position` | **Omit** | Including it turns this back into a standard alert |

## Behaviour

Every order is sent as a plain buy or sell, with no reduce-only marking.
Tickerly does not distinguish entries from exits.

The practical effect is that your strategy is fully responsible for sizing. If
you hold 1 BTC long and want to end up 1 BTC short, the alert must send a sell
of 2 BTC. A strategy written for swing trading normally does this already.

## Align your position before you start

!!! warning "Open the matching position first"
    Swing strategies are almost always in a position. Before enabling
    automation, open the same position manually on your exchange that your
    strategy currently holds, so the two start aligned.

    If you skip this, the first swing will size against a position you do not
    have, and your exchange and your strategy will stay out of step until you
    reset them.

## Exchange support

All supported exchanges.

## Constraints

**Cannot be combined with hedge mode.** Hedge mode handling requires
`prev_position`, and a swing alert is defined by omitting it. An alert with
both is processed as an ordinary alert and the hedge instruction is ignored,
without an error.

**Not needed on MetaTrader.** On MetaTrader 4 and 5 the standard alert already
handles direct long-to-short transitions. Use the standard format there.

**No exit protection.** Because nothing is reduce-only, an oversized exit will
open a position in the opposite direction rather than being discarded. Only
use this format for strategies that genuinely need it.

## See also

- [Entries, exits and positions](../positions.md)
- [Choosing an alert type](../choosing.md)
