# Module 4 Prompts — Backtesting & ML Training Loop (Hermes Edition)

---

## Section 4.3 — Running Your First Backtest

Replace the EMA periods and logic with your own strategy from Module 3.

```
Write a Python script called backtest.py that does the following:
1. Fetch 24 months of daily OHLCV data for SPY using Alpaca MCP (or yfinance as fallback).
2. Apply the strategy rules from {STRATEGY_FOLDER}/strategies/my-strategy.json.
3. Run the backtest on $10,000 starting capital.
4. Print these stats: total return, Sharpe ratio, max drawdown, win rate, average win, average loss, expectancy in R, total trade count.
5. Save an equity curve chart as {STRATEGY_FOLDER}/data/equity_curve.png.
6. Save detailed trade log as {STRATEGY_FOLDER}/data/backtest_trades.csv.

Save the script to {STRATEGY_FOLDER}/src/backtest.py and run it.
```

**What to record (your baseline):**

```csv
test_number, ema_fast, ema_slow, stop_pct, timeframe, sharpe, expectancy, max_dd, win_rate, trade_count
1, 10, 30, 1.0%, daily, [result], [result], [result], [result], [result]
```

Store this in Memory under key `trading/backtest-baseline` for later comparison.

---

## Section 4.4 — The Iteration Loop

Make one change at a time. Hermes tracks all runs in Memory.

**Iteration: change EMA periods**

```
Read {STRATEGY_FOLDER}/src/backtest.py. Change the fast EMA from 10 to 8 and the slow
EMA from 30 to 21. Keep all other parameters the same. Re-run the backtest.
Append results to the baseline record in Memory.
```

**Iteration: add RSI filter**

```
Read backtest.py. Add an RSI filter: only take long trades when the 14-period RSI
is above 50 at the time of the EMA crossover. Keep all other parameters unchanged.
Re-run and append results.
```

**Rule:** If Sharpe drops, trade count falls below 50, or drawdown rises — revert
immediately. One variable at a time. Hermes can `patch` the script back if needed.

---

## Section 4.5 — Building the Automated ML Training Loop (Parameter Sweep)

```
Write a Python script called sweep.py that:
1. Runs all combinations of:
   - fast EMA periods: [6, 8, 10, 12, 15]
   - slow EMA periods: [18, 21, 25, 30, 35]
   - stop loss percentages: [0.5, 0.75, 1.0, 1.25]
   Skip combinations where fast >= slow.
2. For each combination, run the backtest and record results in a SQLite database
   at {STRATEGY_FOLDER}/data/backtest_results.db.
   Table columns: run_id, fast_ema, slow_ema, stop_pct, sharpe, expectancy,
   max_drawdown, win_rate, trade_count, total_return, run_timestamp.
3. After all combinations complete, print a ranked results table sorted by Sharpe
   ratio, highest first. Show top 20.
4. Save top 10 parameter sets to Memory under key `trading/top-params`.

Use yfinance for data. Print progress as it runs. Save the script to
{STRATEGY_FOLDER}/src/sweep.py and execute it.
```

**Out-of-sample validation:**

```
Modify sweep.py to use only the first 16 months of data for parameter optimisation
(in-sample). Then take the top 10 parameter combinations from in-sample and run
them on the remaining 8 months (out-of-sample). Report both Sharpe values for each.
Save the updated script as sweep_oos.py and run it.
```

---

## Section 4.6 — The Automated Learning Loop

This builds the self-improving research loop: paper trade → log results → retrain → update.

```
Build a trading system learning loop in Python. Four components:

1. Trade logger: append completed paper trades to a trade_log table in
   {STRATEGY_FOLDER}/data/backtest_results.db. Fields: trade_id, timestamp,
   instrument, direction, entry_price, exit_price, pnl_r, fast_ema, slow_ema, stop_pct.

2. Parameter store: functions to read/write current active parameters. Log every
   parameter update to parameter_history table.

3. Retrain trigger: a Hermes cron job (not Python APScheduler!) that fires when
   trade_log reaches a multiple of 100 new trades since last retrain, or every
   Sunday at midnight AEST — whichever comes first. When it fires, run sweep.py
   using current data.

4. Result comparator: after each sweep, compare top out-of-sample Sharpe against
   current active Sharpe. If new Sharpe is >5% higher, update parameter store.
   Log every comparison to retrain_log regardless of outcome.

Wire all four into {STRATEGY_FOLDER}/src/learning_loop.py. It should start with
active parameters from parameter_history and log to the database.
```

**Query the results database (run anytime):**

```
Read {STRATEGY_FOLDER}/data/backtest_results.db using sqlite3 and print:
- All retrain attempts this week
- Whether parameters were updated
- Current active parameters
```

---

**Schedule the learning loop:**

Use `cronjob create` with:
- name: `trading-learning-loop`
- schedule: `0 0 * * 0` (midnight Sunday AEST)
- prompt: "Load skill trading-learning-loop. Run learning_loop.py."
- toolsets: `["file", "terminal", "memory"]`
