# Module 1 Prompts — Setup & Foundation (Hermes Edition)

These are the Hermes prompts used in Module 1. Use them in order as you follow the course.

---

## Section 1.1 — Create Your Strategy Folder

Before anything else, give your strategy a name. Tell Hermes:

```
I want to name my trading strategy "[YOUR_STRATEGY_NAME]".
Create a directory at ~/trading/[YOUR_STRATEGY_NAME]/ and inside it create:
- src/ for Python scripts
- strategies/ for JSON strategy definitions
- logs/ for trade and error logs
- data/ for SQLite databases and CSVs
- config/ for risk thresholds and credentials
- tests/ for backtest scripts
Also create a README.md inside the folder.
```

**Naming rules:** use only lowercase letters, numbers, hyphens, and underscores. No spaces.

For the rest of this course, `{STRATEGY_FOLDER}` means `~/trading/[YOUR_STRATEGY_NAME]/`.

---

## Section 1.2 — Hermes Config & MCP Check

Before building, verify your tools are connected. Run these commands in Hermes:

```
Check if Alpaca MCP is connected by running mcp_alpaca_get_account_info.
Then check TradingView MCP by running mcp_tradingview_yahoo_price on SPY.
Report back whether both are working. If either fails, tell me how to fix my ~/.hermes/config.yaml.
```

**What to expect:** You should see your Alpaca account info and a list of TradingView resources. If either fails, Hermes will guide you to the correct config.

---

## Section 1.3 — Connecting to Alpaca

Once your Alpaca MCP is confirmed working (Section 1.2), verify you can read your account:

```
Run mcp_alpaca_get_account_info and print:
- Account ID
- Cash balance
- Portfolio value
- Whether this is a paper or live account.
```

**What to expect:** You should see your paper account cash balance. If the call fails, Hermes will check the MCP config.

**If you see errors:** Hermes will read `~/.hermes/config.yaml` to verify the Alpaca API key is correctly set in the MCP server block. Common causes: wrong key format, missing env var, or the MCP server entry is missing entirely.
