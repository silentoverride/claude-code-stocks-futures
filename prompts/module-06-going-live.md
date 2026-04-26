# Module 6 Prompts — Going Live (Hermes Edition)

Module 6 is about configuration, risk rules, and the switch to live trading.

---

## Section 6.1 — Risk Management Rules

Tell Hermes your risk parameters and ask it to enforce them:

```
Add the following risk management rules to the trading system:

1. Position sizing: risk exactly 1.5% of current account balance per trade.
   Read the account balance via mcp_alpaca_get_account_info before each trade.

2. Daily loss cap: if total realised losses since market open exceed 5% of
   opening account value, block all new entries for the rest of the day.
   Log "Daily loss cap reached. New trades blocked." when triggered.

3. Kill switch: add a command-line flag --stop-trading that halts all new
   order placement immediately. Open positions are left to their stops.
   Also support a Memory flag: if I set memory key trading/kill-switch to "true",
   the execution layer must refuse any new orders.

4. Max open positions: never have more than [N] positions open at once.
```

Adjust the percentages to match your own risk tolerance.

---

## Section 6.2 — Switching to Live Credentials

**Hermes does not use .env files for credentials.** API keys are stored in the MCP
server config (`~/.hermes/config.yaml`) or in Memory. The switch to live trading
is simpler:

1. Ensure your `~/.hermes/config.yaml` MCP server block points to LIVE Alpaca keys:

```yaml
mcp_servers:
  alpaca:
    env:
      ALPACA_API_KEY: [LIVE_KEY]
      ALPACA_SECRET_KEY: [LIVE_SECRET]
```

2. Verify you're on the live endpoint by running:

```
Run mcp_alpaca_get_account_info. Print whether this is a PAPER or LIVE account.
If it says PAPER, update the MCP config to live keys and try again.
```

**Checklist before switching:**
- [ ] Risk management rules in place (daily loss cap, kill switch, max positions)
- [ ] System tested on paper with at least 10 automated trades
- [ ] All 10 paper trades logged correctly in {STRATEGY_FOLDER}/logs/orders.log
- [ ] Starting with smallest possible position size
- [ ] You know how to use the kill switch (--stop-trading or memory flag)
- [ ] Alpaca live account identity verification completed

---

## Section 6.3 — First Live Trade

The system logic is identical to paper. Only the Alpaca endpoint and keys differ.

```
Before the first live trade, run a final dry-run:
Read {STRATEGY_FOLDER}/src/execution_layer.py and execute with --dry-run.
Verify the calculated position size is the smallest allowed by Alpaca.

If correct, remove --dry-run and place the first live trade.
After execution, use mcp_alpaca_get_orders to confirm the order status.
```

Watch the first few trades manually from the Alpaca live dashboard
(`https://app.alpaca.markets/`) before leaving the system unattended.
