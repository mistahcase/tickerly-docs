# Ticker formats

TradingView and exchanges rarely name the same instrument the same way.
Tickerly converts the symbol from your alert into the form your exchange
expects, so in most cases you can leave `{{ticker}}` as it is and the order
places correctly.

For example, TradingView sends a Bybit perpetual as `BTCUSDT.P`, while Bybit's
own naming is `BTCUSDT`. Tickerly handles the difference.

## Checking what was actually used

Your [alert log](https://tickerly.net/alert-log/) shows the symbol in the
exchange's format, after conversion. If you send `EURUSD` from a TradingView
chart and your log shows `EUR_USD`, the conversion worked.

This makes the log the quickest way to confirm a symbol resolved correctly.
When an alert is rejected for an unknown or unavailable symbol, comparing the
log entry against the instrument list on your exchange usually shows the
problem immediately.

## Overriding the symbol

When automatic conversion does not produce what you want, set the ticker
explicitly:

```json
"ticker": "BTCUSDT"
```

Write the symbol exactly as your exchange lists it. Common reasons to
override:

- Your chart uses a spot pair but you want to trade the perpetual, or the
  reverse.
- Your broker appends a suffix to the instrument name, such as `EURUSD.pro`.
- TradingView's data provider and your exchange list the instrument
  differently.
- You want one strategy to trade a different instrument from the one it was
  backtested on.

## Symbols must exist on your exchange

Tickerly places orders on the exchange your pointer belongs to. A symbol that
exists on TradingView but not on that exchange — or that exists but is not
available to your account, your region, or your account type — is rejected by
the exchange.

!!! tip "Availability varies by country and account"
    Exchanges frequently offer different instruments in different countries,
    and demo accounts often list fewer instruments than live ones. If an
    instrument trades on your exchange's website but your orders are rejected,
    check that it is available to your specific account before assuming the
    symbol is wrong.
