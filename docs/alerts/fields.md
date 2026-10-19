# Field reference

Every field Tickerly accepts in an alert message.

| Field | Required | Accepted values |
|---|---|---|
| [`ticker`](#ticker) | Always | Any symbol available on your exchange |
| [`pointer`](#pointer) | Always | The pointer for your exchange connection |
| [`action`](#action) | Always, except close all | `buy`, `sell` |
| [`quantity`](#quantity) | Always, except close all | A number |
| [`prev_position`](#prev_position) | Standard, hedge, full trade, directional | `flat`, `long`, `short` |
| [`hedgemode`](#hedgemode) | Hedge mode alerts | `hedge` |
| [`takeprofit`](#takeprofit-and-stoploss) | Full trade alerts | A price |
| [`stoploss`](#takeprofit-and-stoploss) | Full trade alerts | A price |
| [`stoplosstype`](#stoplosstype) | Optional, Capital.com only | `normal` |
| [`closeall`](#closeall) | Close all alerts | `true` |

An alert missing a required field is rejected and no order is placed. The
rejection appears in your [alert log](https://tickerly.net/alert-log/).

---

## `ticker`

The symbol to trade. Required on every alert.

```json
"ticker": "{{ticker}}"
```

The `{{ticker}}` placeholder uses the symbol on your TradingView chart.
Tickerly converts it into the format your exchange expects, so you do not
need to match the exchange's naming yourself — see
[Ticker formats](tickers.md).

You can override it with a fixed value to run a strategy on one symbol while
trading another:

```json
"ticker": "BTCUSDT"
```

This is useful when your chart uses a spot pair but you want to trade the
equivalent perpetual, or when TradingView and your exchange list the same
instrument under different names.

---

## `pointer`

Identifies which of your exchange connections places the order. Required on
every alert.

```json
"pointer": "m1K3Y"
```

Each exchange connection you create gets its own pointer. Find them under
[My exchanges](https://tickerly.net/view-my-exchanges/), where each connection
also has a ready-made alert message with the pointer already filled in.

This is the only field you must change from the standard message.

!!! tip
    The pointer determines the destination, not the strategy. You can run any
    number of strategies through the same pointer — there is no per-strategy
    setup on the Tickerly side.

---

## `action`

What the exchange should do. Required on every alert except
[close all](types/close-all.md).

```json
"action": "{{strategy.order.action}}"
```

Accepts `buy` or `sell`, in any capitalisation. The placeholder fills this
from your strategy.

`action` on its own does not mean "open" or "close" — a `sell` might open a
short or close a long. What it does depends on `prev_position`; see
[Entries, exits and positions](positions.md).

!!! warning "Only `buy` and `sell` are valid"
    Values such as `close`, `close_all`, `short`, `long` or `exit` are
    rejected. To close a position without knowing its size, use a
    [close all alert](types/close-all.md).

---

## `quantity`

How much to trade. Required on every alert except
[close all](types/close-all.md), where it is ignored.

```json
"quantity": "{{strategy.order.contracts}}"
```

Must be a number. The placeholder sends the exact size your strategy
calculated, so if your strategy sizes in currency, TradingView converts that
into the instrument's own units before the alert is sent.

You can set a fixed size instead:

```json
"quantity": "0.1"
```

### Units

Quantity is expressed in the units TradingView uses for the instrument, not
in lots or in account currency.

| Instrument type | Unit | Example |
|---|---|---|
| Crypto | Base asset | `0.1` = 0.1 BTC |
| Forex | Units of the base currency | `10000` = 0.1 lot |
| Indices, commodities, stocks | Contracts or shares | `4` = 4 contracts |

For forex, 100,000 units is one standard lot. A strategy trading 0.01 lots
sends `1000`.

!!! warning "Check the minimum order size"
    Every exchange sets a minimum order size per instrument, and some set a
    minimum order *value*. An order below it is rejected by the exchange, not
    by Tickerly, so it appears in your alert log as an exchange rejection.

---

## `prev_position`

The position your strategy held immediately before this alert fired. Required
for standard, hedge mode, full trade and directional alerts. Omitted entirely
for [swing](types/swing.md) and [close all](types/close-all.md) alerts.

```json
"prev_position": "{{strategy.prev_market_position}}"
```

Accepts `flat`, `long` or `short`, in any capitalisation.

This is what lets Tickerly tell an entry from an exit. A `sell` with
`prev_position` of `long` closes a long; the same `sell` with `flat` opens a
short. Tickerly marks closing orders as reduce-only so they cannot accidentally
flip your position.

See [Entries, exits and positions](positions.md) for the full behaviour.

---

## `hedgemode`

Enables hedge mode handling, so a strategy can hold long and short positions
on the same symbol at once. Required only for
[hedge mode alerts](types/hedge-mode.md).

```json
"hedgemode": "hedge"
```

The only accepted value is `hedge`.

!!! warning "`hedgemode` requires `prev_position`"
    Hedge mode handling only applies when `prev_position` is also present. An
    alert with `hedgemode` but no `prev_position` is processed as an ordinary
    alert, with no error — the hedge instruction is simply not applied. This
    is why hedge mode and [swing alerts](types/swing.md) cannot be combined.

Your exchange must also be set to hedge mode. See
[Hedge mode alerts](types/hedge-mode.md) for supported exchanges and setup.

---

## `takeprofit` and `stoploss`

Price levels attached to an entry order, so the position closes on its own
without a further alert. Both are required for
[full trade alerts](types/full-trade.md) and must be sent together.

```json
"takeprofit": "43324",
"stoploss": "41225"
```

Each is an absolute price, not a distance or a percentage. The values are sent
to your exchange with the order, and the exchange manages the exit from that
point on.

Supported on Bybit, Binance, Oanda and Capital.com.

---

## `stoplosstype`

Switches Capital.com from a guaranteed stop loss to a normal one. Optional,
and ignored on every other exchange.

```json
"stoplosstype": "normal"
```

Capital.com defaults to a guaranteed stop loss, because some countries require
one. If normal stops are permitted where you trade and you would rather use
them, include this field.

---

## `closeall`

Closes the entire position for a symbol, whatever its size or direction.
Required for [close all alerts](types/close-all.md).

```json
"closeall": "true"
```

The only accepted value is `true`, in lowercase. `True` and `TRUE` are not
recognised.

When `closeall` is set, `action` and `quantity` are not needed and any values
you send for them are ignored — Tickerly reads the position from your exchange
and closes it. Send only `ticker`, `closeall` and `pointer`.

Not supported on Alpaca or MetaTrader.
