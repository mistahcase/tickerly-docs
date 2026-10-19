# API keys and permissions

Tickerly places orders through an API key you create at your exchange. This
page covers what that key needs — and, just as importantly, what it should
never have.

## Permissions to enable

| Permission | Setting | Why |
|---|---|---|
| Trading | **Enable** | Required. Without it, no orders can be placed. |
| Reading account data | **Enable** | Needed to check balances and positions, and for [close all alerts](../alerts/types/close-all.md). |
| Withdrawals | **Leave disabled** | Never required. |

!!! danger "Never enable withdrawal permission"
    Tickerly does not move funds and has no feature that needs withdrawal
    access. An API key that cannot withdraw cannot be used to take your money,
    whatever happens to the key.

    If your exchange enables withdrawals by default when creating a key, turn
    it off before saving.

Some exchanges also offer margin, futures or sub-account transfer permissions
as separate toggles. Enable those only for the markets you actually intend to
trade — futures trading permission is needed for a futures connection, for
example, but spot-only traders can leave it off.

## IP whitelisting

Where your exchange supports restricting an API key to specific addresses, add:

```
217.160.238.76
```

This is the only address Tickerly places orders from. Whitelisting means a
leaked key is unusable from anywhere else, and it is the single most effective
thing you can do to secure the connection.

!!! warning "Whitelisting is all or nothing"
    If you whitelist any address, you must include the one above or every
    order will be rejected. If orders stopped placing immediately after you
    changed API security settings, check this first.

Some exchanges expire un-whitelisted keys automatically after a period —
Binance does this — so whitelisting also keeps the connection from lapsing.

## Saving your credentials

Most exchanges show the API secret exactly once, at creation. Copy every value
before closing the page: the key, the secret, and any passphrase, password or
account identifier the exchange generates.

If you lose the secret, you cannot recover it. Delete the key at the exchange
and create a new one.

## Keeping a key working

A connection that worked and then stopped is usually one of these:

- **The key expired.** Some exchanges expire keys on a schedule, or after a
  period without whitelisting.
- **The IP whitelist changed.** Either the address above was removed, or
  whitelisting was switched on without it.
- **Permissions were edited.** Trading or reading was turned off.
- **The key was deleted.** Some exchanges revoke all keys when the account
  password changes or when 2FA is reset.

In each case the fix is the same: create a fresh key at the exchange and
update the credentials on your Tickerly connection.

## Next

- [Credential fields by exchange](credentials.md) — where each value goes
