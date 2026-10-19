# The alert message

Every trade Tickerly places starts as an alert message sent from TradingView.
The message is a small block of JSON that you paste into the **Message** field
when creating a TradingView alert.

A standard alert looks like this:

```json
{
"ticker": "{{ticker}}",
"action": "{{strategy.order.action}}",
"prev_position" : "{{strategy.prev_market_position}}",
"quantity": "{{strategy.order.contracts}}",
"pointer" : "replace with pointer for your exchange here"
}
```

The only part you must change is `pointer`. Copy the ready-made message for
your exchange from [My exchanges](https://tickerly.net/view-my-exchanges/) and
the pointer is already filled in.

## Where the alert is sent

Alerts go to Tickerly's alert processing server:

```
http://alerts.tickerly.net
```

Set this under the **Notifications** tab of the TradingView alert, with
**Webhook URL** ticked.

!!! note "The endpoint is HTTP, not HTTPS"
    This is deliberate. Alert messages contain no account credentials — only a
    ticker, an order instruction and your pointer — and plain HTTP shaves
    latency off every signal. Your exchange API keys are never part of an
    alert; they stay in your Tickerly exchange connection.

All orders Tickerly places on your exchange originate from a single address:

```
217.160.238.76
```

Add it to your exchange's API whitelist where the exchange offers one.

## How values get filled in

Each value in the message is either a **placeholder** that TradingView
replaces when the alert fires, or a **fixed value** you write yourself.

| | Example | Result |
|---|---|---|
| Placeholder | `"ticker": "{{ticker}}"` | The chart's current symbol |
| Fixed value | `"ticker": "BTCUSDT"` | Always BTCUSDT |

Any field can take either. Mixing is fine — a common pattern is a placeholder
for the action and a fixed value for the quantity.

!!! warning "Placeholders only work in strategy alerts"
    TradingView fills in `{{strategy.order.action}}` and
    `{{strategy.prev_market_position}}` only when the alert is attached to a
    **strategy**. On an indicator, those placeholders arrive at Tickerly
    unsubstituted and the alert is rejected. If you are automating an
    indicator, use fixed values — see
    [Directional alerts](types/directional.md).

## Structure rules

- The message must be valid JSON. TradingView shows a green checkmark in the
  alert message box when it is.
- All values are written as strings, in double quotes, including numbers.
- Field names are lowercase. `closeall` must also be written lowercase as
  `"true"`.
- `action` and `prev_position` values are not case-sensitive: `buy` and `BUY`
  both work.
- Unknown fields are ignored.

## What happens next

When an alert arrives, Tickerly checks it, works out whether the order opens
or closes a position, converts the ticker into the format your exchange
expects, and places the order. The exchange confirms or rejects it, and the
result appears in your
[alert log](https://tickerly.net/alert-log/) and in your notifications.

Nothing about this is configured per strategy. Once your exchange is
connected, every alert carrying that pointer is processed the same way.

## Next

- [Field reference](fields.md) — every field, its values and when it is required
- [Entries, exits and positions](positions.md) — how Tickerly decides to open or close
- [Choosing an alert type](choosing.md) — which of the six formats you need
