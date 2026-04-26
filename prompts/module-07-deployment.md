# Module 7 Prompts — Automation & Resilience (Hermes Edition)

---

## Section 7.1 — Deployment Options

**Option A: Hermes Cron Jobs (recommended)**
Instead of PM2, use Hermes' built-in cron scheduler to run the bot on a schedule.

**Option B: Long-Running Python Process**
If you need a continuous websocket listener, Hermes can start a background process
via `terminal(background=true)`.

---

## Section 7.2 — Hermes Cron Job Setup

Create a cron job that runs the trading system automatically:

```
Create a Hermes cron job with these parameters:
- name: trading-system-main
- schedule: */15 * * * 1-5 (every 15 minutes, Mon-Fri, market hours)
- prompt: "Load the trading system skill. Run {STRATEGY_FOLDER}/src/signal_engine.py
  and {STRATEGY_FOLDER}/src/execution_layer.py in sequence. Risk 1% per trade.
  Only trade if signal fires. Log everything."
- toolsets: ["alpaca", "tradingview", "file", "memory"]
```

Verify it was created: `cronjob action=list`

**Key cron commands:**

```
# Check all cron jobs and their status
cronjob action=list

# Pause the bot (kill switch)
cronjob action=pause job_id=trading-system-main

# Resume
cronjob action=resume job_id=trading-system-main

# Remove entirely
cronjob action=remove job_id=trading-system-main
```

---

## Section 7.3 — Telegram Alerts

**Step 1:** Create a bot via `@BotFather` in Telegram. Send `/newbot`, follow prompts, copy token.

**Step 2:** Get your chat ID:
`https://api.telegram.org/bot<your-token>/getUpdates`

**Step 3:** Store credentials in Memory (not .env!):

```
Save these to memory:
- telegram/bot_token: [your token]
- telegram/chat_id: [your chat ID]
```

**Step 4:** Add Telegram notifications to the system:

```
Modify {STRATEGY_FOLDER}/src/execution_layer.py. Add a function send_telegram_alert(message).
It should:
- Read telegram/bot_token and telegram/chat_id from Memory.
- Use the Telegram Bot API directly with a simple HTTP POST (requests or urllib).
- Send a notification when:
  1. A trade is opened (instrument, direction, size, entry price)
  2. A trade is closed (P&L in dollars)
  3. A stop loss is hit
  4. Any unhandled error occurs
```

**Add a daily summary (optional):**

```
Create a second cron job:
- name: trading-daily-summary
- schedule: 0 17 * * 1-5 (5 PM ET, after market close)
- prompt: "Read {STRATEGY_FOLDER}/logs/orders.log. Summarise today's trades:
  count, total P&L, win rate. Send as Telegram message."
- toolsets: ["file", "memory"]
```

---

## Section 7.4 — Error Handling & Reconnection Logic

```
Add error handling and automatic reconnection to the trading system:

1. If the Alpaca websocket connection drops, automatically attempt to reconnect
   every 30 seconds. Log each attempt to {STRATEGY_FOLDER}/logs/errors.log.

2. If an order placement fails via mcp_alpaca_place_stock_order:
   - Retry once after 5 seconds.
   - If it fails again, send a Telegram alert and skip the trade.
   - Do NOT retry indefinitely.

3. Wrap the main trading loop in a try/except. If an unhandled exception occurs:
   - Log full stack trace to errors.log.
   - Send Telegram alert with error message.
   - Restart the loop after 60 seconds.

4. Log all events (signals, orders, errors, reconnections) to
   {STRATEGY_FOLDER}/logs/trading-system.log with timestamps.

Save the updated execution_layer.py.
```
