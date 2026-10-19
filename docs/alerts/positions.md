# Entries, exits and positions

Tickerly works out whether an order opens, adds to, or closes a position by
reading `action` together with `prev_position`. Neither field means anything
on its own.

This page describes exactly what each combination does. It is the single most
useful thing to understand if an order behaved differently from how you
expected.

## The six combinations

| `action` | `prev_position` | What happens |
|---|---|---|
| `buy` | `flat` | Opens a long |
| `buy` | `long` | Adds to your long |
| `sell` | `long` | **Closes your long** |
| `sell` | `flat` | Opens a short |
| `sell` | `short` | Adds to your short |
| `buy` | `short` | **Closes your short** |

Only two of the six close a position: selling when you were long, and buying
when you were short. Everything else opens or increases one.

## Closing orders are reduce-only

When Tickerly identifies a closing order, it marks it reduce-only at the
exchange. That order can shrink your position or flatten it, but it cannot
open a new position in the opposite direction.

This is a safety property. If your strategy sends an exit for more than you
actually hold — because the exchange rounded your entry, or because a partial
fill left you with less than expected — the surplus is discarded instead of
opening an unwanted position in the other direction.

It also has a consequence worth knowing about: a strategy that goes directly
from long to short in one alert cannot be automated with a standard alert,
because the closing order refuses to cross through zero. That is what
[swing alerts](types/swing.md) are for.

## When `prev_position` is missing

If `prev_position` is not in the alert, Tickerly treats every order as an
entry. No order is marked reduce-only, and `buy` and `sell` are sent to the
exchange as plain instructions.

This is exactly what a [swing alert](types/swing.md) is, and it is why a swing
alert can flip a position from long to short in one order.

It also means an accidentally omitted `prev_position` will not produce an
error. The orders will place — they will just never be treated as exits. If a
strategy's positions are growing when they should be closing, check that
`prev_position` is present and being filled in.

## In hedge mode

With [hedge mode](types/hedge-mode.md), the same six combinations apply, but
the direction of the position is carried through to the exchange so that long
and short positions on the same symbol coexist rather than cancelling out.

| `action` | `prev_position` | Affects |
|---|---|---|
| `buy` | `flat` | Opens on the long side |
| `buy` | `long` | Adds to the long side |
| `sell` | `long` | Closes the long side |
| `sell` | `flat` | Opens on the short side |
| `sell` | `short` | Adds to the short side |
| `buy` | `short` | Closes the short side |

A sell that closes a long touches only the long side and leaves any short
position untouched. This is what lets two strategies run on the same symbol
without interfering with each other.

## Keeping your strategy and your exchange aligned

Tickerly places the order your alert describes. It does not compare your
strategy's idea of your position against what your exchange actually holds.

If the two drift apart — because you opened a trade manually, because you
started automating mid-trade, or because an earlier order was rejected — the
alerts will keep describing positions you do not have. The usual symptom is a
run of exit orders rejected for having no position to close.

Two habits prevent most of this:

- Start a strategy when it is flat, so Tickerly and your exchange begin from
  the same state. Most strategies show the current position in the TradingView
  Strategy Tester.
- If your strategy is always in a position, open the matching position
  manually on your exchange before enabling the alert.

If they do drift, a [close all alert](types/close-all.md) or a manual close on
the exchange resets you to flat, and you can restart the strategy from there.
