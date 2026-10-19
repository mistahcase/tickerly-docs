# Credential fields by exchange

Exchanges do not agree on what to call their API credentials. The Tickerly
form uses one set of field names for all of them, so the value your exchange
calls a "personal token" may belong in a box labelled **API SECRET**.

This page says which value goes where.

## The Tickerly form

| Tickerly field | Holds |
|---|---|
| **EXCHANGE ACCOUNT NAME** | A label of your choosing. Must contain `test` for a demo account — see [Demo and test accounts](demo-accounts.md). |
| **API KEY / ACCOUNT ID** | The public identifier |
| **API SECRET / API ACCESS TOKEN / API USER NAME** | The secret value |
| **API PASSWORD / API PASSPHRASE** | A third value, where the exchange uses one |
| **ACCOUNT ID** | An additional account identifier, where the exchange uses one |

Not every exchange uses every field. Leave unused fields empty.

## Mapping

=== "Oanda"

    | Oanda calls it | Enter it in |
    |---|---|
    | v20 Account Number | API KEY / ACCOUNT ID |
    | Personal token | API SECRET / API ACCESS TOKEN / API USER NAME |

=== "Capital.com"

    | Capital.com calls it | Enter it in |
    |---|---|
    | API Key | API KEY / ACCOUNT ID |
    | Account email address | API SECRET / API ACCESS TOKEN / API USER NAME |
    | Custom Password (API) | API PASSWORD / API PASSPHRASE |

    The custom password is the one you set when creating the API key, not your
    Capital.com login password.

=== "Alpaca"

    | Alpaca calls it | Enter it in |
    |---|---|
    | Key | API KEY / ACCOUNT ID |
    | Secret | API SECRET / API ACCESS TOKEN / API USER NAME |

=== "Binance"

    | Binance calls it | Enter it in |
    |---|---|
    | API Key | API KEY / ACCOUNT ID |
    | Secret Key | API SECRET / API ACCESS TOKEN / API USER NAME |

    The same mapping applies to Binance, Binance COIN-M, Binance USD-M and
    Binance US. Choose the connection type that matches the market you intend
    to trade.

=== "Bybit"

    | Bybit calls it | Enter it in |
    |---|---|
    | APIKey | API KEY / ACCOUNT ID |
    | API Secret | API SECRET / API ACCESS TOKEN / API USER NAME |

    Enable sub-account or account transfer when creating the key if you intend
    to trade from a sub-account.

=== "Bitget"

    | Bitget calls it | Enter it in |
    |---|---|
    | API key | API KEY / ACCOUNT ID |
    | Secret key | API SECRET / API ACCESS TOKEN / API USER NAME |
    | Passphrase (API token) | API PASSWORD / API PASSPHRASE |

=== "Kraken"

    | Kraken calls it | Enter it in |
    |---|---|
    | API key | API KEY / ACCOUNT ID |
    | Private key | API SECRET / API ACCESS TOKEN / API USER NAME |

    The same mapping applies to Kraken, Kraken Margin and Kraken Futures.

=== "KuCoin"

    | KuCoin calls it | Enter it in |
    |---|---|
    | API Key | API KEY / ACCOUNT ID |
    | API Secret | API SECRET / API ACCESS TOKEN / API USER NAME |
    | API Passphrase | API PASSWORD / API PASSPHRASE |

    The passphrase is the one you chose when creating the key, not your
    KuCoin login password. The same mapping applies to KuCoin and
    KuCoin Futures.

=== "Hyperliquid"

    | Hyperliquid calls it | Enter it in |
    |---|---|
    | API wallet address | API KEY / ACCOUNT ID |
    | API wallet private key | API SECRET / API ACCESS TOKEN / API USER NAME |
    | Master wallet address | ACCOUNT ID |

    Hyperliquid uses an API wallet that is separate from the wallet holding
    your funds. Both addresses are needed: the API wallet signs the orders,
    and the master wallet is the account they are placed against.

    !!! warning "Fill in the master wallet address"
        A connection without it still places ordinary orders, so the setup
        looks correct. [Close all alerts](../alerts/types/close-all.md) are
        the only thing that fails, because they need to read your open
        position from the master wallet.

        If close all is the only thing not working on Hyperliquid, this is
        why.

    The same mapping applies to Hyperliquid Spot and Hyperliquid Perps.

=== "MetaTrader"

    MetaTrader connections use no API credentials. You give the connection a
    name, Tickerly generates a pointer, and that pointer goes into the
    connector running in your terminal.

    See [MetaTrader setup](https://tickerly.net/connect-to-metatrader/).

## Getting it wrong

Credentials are not validated when you save the connection — the first sign
of a mistake is usually an alert rejected for an invalid key or missing
permission.

If that happens, check the mapping above before assuming the key itself is
broken. Transposing the secret and the passphrase is the most common error,
because several exchanges generate all three values on the same screen.

## Next

- [Demo and test accounts](demo-accounts.md)
- [Position mode](position-mode.md)
