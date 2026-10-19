# Undertow Alerts

A Telegram bot that warns you before a [Perpl](https://perpl.xyz) position, or a large liquidation zone, becomes dangerous. It is a companion to [Undertow](https://github.com/dhruvjnv72/undertow), a read-only risk terminal for Perpl on Monad.

**Status: in progress, started October 2026.** Nothing is deployed yet. This README describes what is planned; the checklist below shows what is done.

## Why

Traders do not stare at a dashboard all day. Undertow already knows where every position gets liquidated; this bot sends that knowledge to the place people actually look: their phone.

## Planned commands

| Command | What it will do |
|---|---|
| `/start` | explain what the bot does and its limits |
| `/watch <wallet or account number> [within %]` | alert me when any position of this wallet gets within the chosen distance of liquidation |
| `/unwatch <wallet or account number>` | stop watching |
| `/zones <market>` | send the biggest liquidation zones near the price right now |
| `/zonealert <market> <min $> <within %>` | alert me when a zone of at least this size gets within this distance of the price |
| `/status` | show what I am watching and how fresh the data is |

## How it will work

1. The bot polls Undertow's JSON API (`/api/wallet`, `/api/radar`) every ~30 seconds.
2. Subscriptions are stored in SQLite.
3. An alert fires when a threshold is crossed, and not again for a cooldown period, so a wallet hovering near a threshold does not spam the chat.
4. Configuration comes from environment variables only: `TELEGRAM_BOT_TOKEN`, `UNDERTOW_API_URL`.

## Milestones

- [ ] Project skeleton, config from environment, tests
- [ ] Client for Undertow's API with timeouts and error handling
- [ ] SQLite subscription store
- [ ] `/watch`, `/unwatch`, `/status`
- [ ] Threshold and cooldown logic, with unit tests
- [ ] `/zones` and `/zonealert`
- [ ] Run it against a live Undertow instance and record a short demo
- [ ] Deploy notes

## Honest limits

- Undertow's positions are a snapshot up to about a minute old, so an alert can arrive late.
- Alerts are information, not financial advice.
- Telegram delivery is not guaranteed.
- The bot only reads public data. It never holds keys or sends transactions.

## Security

The bot token is a secret. It is never committed; `.env` is git-ignored.

## License

MIT, see [LICENSE](LICENSE).
