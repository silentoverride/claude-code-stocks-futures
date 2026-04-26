# Module 8 Prompts — Expansion & Advanced Moves (Hermes Edition)

---

## Section 8.1 — Running Multiple Markets in Parallel

**Step 1:** Parameterise the instrument so the same script can run any market:

```
The current trading system runs for a single instrument hardcoded as SPY.
Extend it so that:
1. The instrument is accepted as a command-line argument.
2. The trade log filename includes the instrument name so SPY and NQ logs
   don't overwrite each other.
3. The cron job name also includes the instrument.

Save the updated script as {STRATEGY_FOLDER}/src/execution_layer.py.
```

**Step 2:** Add a second instrument as a separate cron job:

```
Create a second cron job:
- name: trading-system-nq
- schedule: */15 * * * 1-5
- prompt: "Run the trading system for instrument NQ=F. Use the same execution
  layer but pass --instrument NQ=F. Risk 1% per trade."
- toolsets: ["alpaca", "tradingview", "file", "memory"]
```

**For more instruments:** Repeat Step 2 with different names and instrument args.

---

## Section 8.2 — Adding a Second Strategy (Mean Reversion)

```
Build a mean reversion strategy. Save it to {STRATEGY_FOLDER}/strategies/mean-reversion.json.

Entry conditions (long):
- RSI(14) on 15-minute chart drops below 30
- Price is more than 1 ATR below the 20-period EMA

Exit conditions (long):
- RSI crosses back above 50, OR
- Stop loss at 1.5 ATR below entry

Short-side mirror:
- Sell when RSI > 70 AND price > 1 ATR above 20 EMA
- Exit when RSI crosses below 50

Write the signal engine for this strategy as
{STRATEGY_FOLDER}/src/signal_engine_meanrev.py.
It should read the same market data but apply these different rules.
```

Add it as a separate cron job:

```
Create a cron job:
- name: trading-system-meanrev-spy
- schedule: */15 * * * 1-5
- prompt: "Run the mean reversion strategy on SPY. Use
  {STRATEGY_FOLDER}/src/signal_engine_meanrev.py and
  {STRATEGY_FOLDER}/src/execution_layer.py together. Risk 1% per trade."
- toolsets: ["alpaca", "tradingview", "file", "memory"]
```

**If a strategy errors at startup:** Read its log file in
`{STRATEGY_FOLDER}/logs/` and diagnose. Common causes: symbol format mismatch,
missing indicator library imports, or incorrect ATR calculation period.

---

## What's Running After Module 8

Multiple Hermes cron jobs:
- `trading-system-spy` — EMA crossover trend strategy, SPY
- `trading-system-nq` — same strategy, NQ futures
- `trading-system-meanrev-spy` — RSI + ATR mean reversion, SPY

Two strategies with genuinely different edges: trend-following wins when markets
are directional; mean reversion wins when they're oscillating. Bad periods don't
overlap the same way.

---

## Going Further (Section 8.3)

Ideas to extend — all achievable with Hermes tools:

**Sentiment layer:**

```
Use web_search to fetch recent headlines for SPY.
Score each headline as bullish/bearish/neutral using a simple keyword list.
Adjust position size: if sentiment is strongly bullish, increase size by 20%.
If strongly bearish, decrease size or skip the trade.
```

**Multi-timeframe confirmation:**

```
Run the signal engine on both 1-hour (trend direction) and 5-minute (entry) data.
Only take entries when both timeframes agree. Fetch both timeframes using
mcp_alpaca_get_stock_bars with different timeframes.
```

**Portfolio-level risk management:**

```
Before placing any new trade, use mcp_alpaca_get_all_positions to check total
open exposure. If total exposure across all strategies exceeds 5% of account
value, pause new entries across ALL cron jobs until exposure reduces.
```

All of these are plain-English descriptions you give to Hermes.
The architecture is already in place.
