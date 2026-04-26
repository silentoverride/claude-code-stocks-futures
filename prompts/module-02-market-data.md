# Module 2 Prompts — Market Data & Signal Generation (Hermes Edition)

---

## Section 2.1 — Connecting to Live Market Data

```
Use mcp_alpaca_get_stock_bars to fetch the last 200 bars of SPY at 15-minute resolution.
Then use mcp_tradingview_get_stock_bars to verify SPY data is available there too.
Print the latest 5 bars (timestamp, open, high, low, close, volume) from Alpaca.
```

**Futures traders:** Replace `SPY` with `NQ=F` (NASDAQ futures) or your futures symbol. Alpaca supports some futures; for others use TradingView MCP data.

**If you see a data error:** Tell Hermes:

```
The symbol [SYMBOL] isn't returning data from Alpaca. Use mcp_tradingview_get_stock_bars to find the correct ticker format, then fetch the same data via TradingView MCP instead.
```

---

## Section 2.2 — Building the Signal Engine

```
Write a Python script called signal_engine.py that does the following:
1. Fetch the last 200 bars of SPY at 15-minute resolution using Alpaca MCP (via a wrapper or direct API call — if MCP doesn't exist, use alpaca-py SDK).
2. Calculate the 20-period EMA and the 50-period EMA using pandas-ta or ta-lib.
3. Fire a BUY SIGNAL when the 20 EMA crosses above the 50 EMA.
4. Fire a SELL SIGNAL when the 20 EMA crosses below the 50 EMA.
5. Print signals in this format: '[BUY SIGNAL] SPY 454.32 — 20 EMA crossed above 50 EMA on 15m'.
6. Maintain a rolling window of at least 100 bars so EMAs are statistically meaningful.

Save it to {STRATEGY_FOLDER}/src/signal_engine.py.
```

**Note:** The engine needs at least 50 completed bars before it can calculate the 50-period EMA. For testing without waiting for live data, use this:

```
Add a backtesting mode that runs the signal engine against the last 200 bars of historical data from Alpaca instead of live data, so I can verify the signal logic is working correctly. Save as signal_engine_backtest.py.
```

---

## Section 2.3 — Customising Your Signal

```
Read {STRATEGY_FOLDER}/src/signal_engine.py. I want to customise the indicators.
Change the fast EMA to [X] periods and the slow EMA to [Y] periods.
Add an RSI filter: only fire a buy signal if 14-period RSI is below [Z] at crossover.
Save the updated script and re-run the backtest mode to verify it still produces signals.
```

Replace [X], [Y], [Z] with your desired values.
