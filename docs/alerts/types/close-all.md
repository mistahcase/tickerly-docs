# Close all alerts

Closes the entire position for a symbol without needing to know its size or
direction.

## Purpose

Ordinary exit alerts must state how much to close. If your strategy's idea of
the position has drifted from what your exchange actually holds, those exits
fail.

A close all alert asks the exchange what is open and closes it. Use it as a
strategy's final exit, as a flatten-everything safety signal, or manually to
reset a symbol to a known state.

## Message

```json
{
"ticker": "{{ticker}}",
"closeall" : "true",
"pointer" : "replace with pointer for your exchange here"
}
```

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `ticker` | Yes | The symbol to flatten |
| `closeall` | Yes | Only accepted value is `true`, lowercase |
| `pointer` | Yes | |
| `action` | Ignored | Any value sent is discarded |
| `quantity` | Ignored | Any value sent is discarded |

!!! warning "`true` must be lowercase"
    `True` and `TRUE` are not recognised. An alert with the wrong
    capitalisation and no `action` field is rejected.

Best practice is to send only the three fields above. Including `action` or
`quantity` does no harm, but they have no effect and make the alert harder to
read.

## Behaviour

Tickerly reads your open position for the symbol from the exchange and closes
it, working out the direction and size itself. In hedge mode, both sides are
closed.

If nothing is open for that symbol, the alert is rejected with a message
saying there is no position to close. That is expected behaviour rather than a
fault — it commonly happens when a strategy sends a final close after an exit
has already flattened the position.

## Exchange support

All supported exchanges **except Alpaca, MetaTrader 4 and MetaTrader 5**.

On those, close positions with an ordinary exit alert stating the size, or
manually.

## Constraints

**It closes everything on that symbol, not just your strategy's share.** If two
strategies hold positions on the same symbol through the same exchange
connection, a close all sent by one closes both. Be careful using it alongside
[hedge mode](hedge-mode.md).

**It does not cancel pending orders.** Stop loss and take profit orders resting
at the exchange, including those set by a
[full trade alert](full-trade.md), may survive the position being closed.
Check your exchange's order list afterwards.

**It applies to one symbol.** There is no alert that flattens every symbol on
an account at once.

**On Hyperliquid, the master wallet address is required.** A Hyperliquid
connection missing it places ordinary orders normally but cannot close all,
because the open position is read from the master wallet. See
[Credential fields by exchange](../../exchanges/credentials.md).

## See also

- [Entries, exits and positions](../positions.md)
- [Send a manual alert](https://tickerly.net/trigger-manual-alert/)
