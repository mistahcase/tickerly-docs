# Hedge mode alerts

For running more than one strategy on the same symbol, or holding long and
short positions at the same time.

## Purpose

In one-way mode, a short order on a symbol you are long simply reduces that
long. Two strategies trading the same symbol will close each other's
positions.

Hedge mode lets long and short positions on the same symbol exist
independently. Each strategy's trades play out without affecting the other.

## Message

```json
{
"hedgemode": "hedge",
"ticker": "{{ticker}}",
"action": "{{strategy.order.action}}",
"prev_position" : "{{strategy.prev_market_position}}",
"quantity": "{{strategy.order.contracts}}",
"pointer" : "replace with pointer for your exchange here"
}
```

## Fields

| Field | Required | Notes |
|-------|----------|-------|
| `hedgemode` | Yes | Only accepted value is `hedge` |
| `ticker` | Yes | |
| `action` | Yes | `buy` or `sell` |
| `prev_position` | Yes | Required — see below |
| `quantity` | Yes | |
| `pointer` | Yes | |

## Behaviour

The direction of the position is carried through to your exchange, so orders
apply to the long side or the short side rather than to a single net position.

| `action` | `prev_position` | Affects |
|---|---|---|
| `buy` | `flat` | Opens on the long side |
| `buy` | `long` | Adds to the long side |
| `sell` | `long` | Closes the long side |
| `sell` | `flat` | Opens on the short side |
| `sell` | `short` | Adds to the short side |
| `buy` | `short` | Closes the short side |

A sell closing a long touches only the long side and leaves any short position
untouched.

!!! warning "`prev_position` is required"
    Hedge mode handling is applied only when `prev_position` is present. An
    alert with `hedgemode` but no `prev_position` is processed as an ordinary
    alert and the hedge instruction is silently ignored — no error is raised.

    If hedged strategies are closing each other's positions, check
    `prev_position` first.

## Set your exchange to hedge mode

Sending hedge alerts is only half of it — your exchange account must be in
hedge mode too, or the orders will be rejected.

Most exchanges default to one-way mode.

=== "Bybit"

    Set per symbol. Open the symbol, select the three dots in the top right,
    choose **Trading Preferences**, and set **Position Mode** to
    **Hedge mode**.

=== "Binance"

    Set for all futures at once. Go to futures, select the three dots in the
    top right, choose **Preferences**, and set **Position Settings** to
    **Hedge Mode**.

=== "Bitget"

    Bitget defaults to hedge mode rather than one-way, so no change is
    normally needed.

=== "Oanda"

    No account setting is required.

=== "MetaTrader"

    Hedging is a property of the account your broker issued. If your account
    is a netting account, your broker must supply a hedging account instead.

## Exchange support

Binance, Bybit, Bitget, Oanda, MetaTrader 4 and MetaTrader 5.

On Oanda the standard alert is enough on its own; including `hedgemode` is
accepted and does no harm.

## Constraints

**Cannot be combined with swing alerts.** See [Swing alerts](swing.md).

**Mismatched modes are rejected.** Sending hedge alerts to an account in
one-way mode, or standard alerts to an account in hedge mode, produces
rejections from the exchange. If orders stopped placing after you changed your
account's position mode, this is usually why.

## See also

- [Entries, exits and positions](../positions.md)
- [Running multiple strategies on one ticker](https://tickerly.net/maximizing-profits-with-hedge-mode-and-multi-strategy-approach/)
