# Directional alerts

Four separate alerts — one for each action a strategy can take — each carrying
fixed values rather than strategy placeholders.

## Purpose

Standard alerts rely on TradingView filling in `{{strategy.order.action}}` and
`{{strategy.prev_market_position}}`. Only strategies supply those. Indicators
do not.

Directional alerts replace them with fixed values, so each alert describes
exactly one action. This is how indicators are automated, and it is also
useful when you want explicit control over each entry and exit.

## Messages

All four use the same fields. Only the values change.

=== "Enter long"

    ```json
    {
    "ticker": "EURUSD",
    "action": "buy",
    "prev_position" : "flat",
    "quantity": "10000",
    "pointer" : "replace with pointer for your exchange here"
    }
    ```

=== "Exit long"

    ```json
    {
    "ticker": "EURUSD",
    "action": "sell",
    "prev_position" : "long",
    "quantity": "10000",
    "pointer" : "replace with pointer for your exchange here"
    }
    ```

=== "Enter short"

    ```json
    {
    "ticker": "EURUSD",
    "action": "sell",
    "prev_position" : "flat",
    "quantity": "10000",
    "pointer" : "replace with pointer for your exchange here"
    }
    ```

=== "Exit short"

    ```json
    {
    "ticker": "EURUSD",
    "action": "buy",
    "prev_position" : "short",
    "quantity": "10000",
    "pointer" : "replace with pointer for your exchange here"
    }
    ```

The examples trade EURUSD at a fixed 10,000 units, which is 0.1 lots.

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `ticker` | Yes | Fixed value, or a placeholder your indicator supplies |
| `action` | Yes | Fixed: `buy` or `sell` |
| `prev_position` | Yes | Fixed: `flat` for entries, `long` or `short` for exits |
| `quantity` | Yes | Fixed, or a placeholder your indicator supplies |
| `pointer` | Yes | |

## Behaviour

Each alert is processed exactly as a standard alert with the same values.
Entries open positions; exits are sent reduce-only. See
[Entries, exits and positions](../positions.md).

The difference is only in where the values come from. Because they are fixed,
you are responsible for making sure each alert is attached to the right
condition in your indicator — Tickerly cannot tell that an "exit long" alert
fired on an entry signal.

## Using strategy input fields

If your strategy has input boxes for custom messages per action, you can put
these messages there and use whatever placeholders the strategy exposes:

```json
{
"ticker": "#symbol#",
"action": "buy",
"prev_position" : "flat",
"quantity": "#quantity#",
"pointer" : "replace with pointer for your exchange here"
}
```

The alert message box then usually contains only:

```
{{strategy.order.alert_message}}
```

Some strategies use `{{strategy.order.comment}}` instead. Test which one your
strategy fills in.

## Exchange support

All supported exchanges.

## Constraints

**Your indicator must be able to fire alerts on the right conditions.** Whether
it can is a property of the indicator's script, not of Tickerly. Many
indicators only signal entries; if yours has no exit condition, you cannot
automate exits with it.

**Fixed sizes do not adapt.** A fixed quantity is the same on every trade
regardless of volatility or account size. If your indicator supplies a size
placeholder, use it.

**Check each alert separately.** Four alerts means four opportunities to paste
the wrong message. Enabling email notifications on the TradingView alert lets
you confirm each one sends what you expect.

## See also

- [Field reference](../fields.md)
- [Entries, exits and positions](../positions.md)
