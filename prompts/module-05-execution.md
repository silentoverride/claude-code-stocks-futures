# Module 5 Prompts — Paper Trading Execution (Hermes Edition)

---

## Section 5.2 — Building the Execution Layer

```
Build an execution layer in Python called execution_layer.py. Save it to
{STRATEGY_FOLDER}/src/execution_layer.py. It must:

1. Listen for buy/sell signals from the signal engine (read from a signal file or
   be imported as a module).
2. When a buy signal fires for SPY:
   - Read account balance via mcp_alpaca_get_account_info (or Alpaca SDK).
   - Calculate position size to risk exactly 1% of account balance.
   - Use stop loss distance to calculate position size.
     Example: $10,000 account, 1% risk = $100. If stop is $2 away from entry,
     position size = $100 / $2 = 50 shares.
   - Place a bracket order: entry (market), stop loss, take profit.
   - Take profit at 2:1 reward-to-risk ratio.
3. Return order confirmation or error message.
4. Log every order attempt to {STRATEGY_FOLDER}/logs/orders.log.
```

**Adjust for your instrument:** Replace `SPY`, the stop distance, and point values
with your own. The position sizing logic is the same regardless of instrument.

---

## Position Sizing Example

For futures (NQ) on Alpaca:
- Account: $10,000
- Risk per trade: 1% = $100
- Stop distance: 47 points
- NQ contract value: $20 per point
- Max contracts: $100 / (47 x $20) = 0.1 contracts

Retail accounts should use MNQ (Micro NASDAQ, $2/point) for realistic sizing.
Adjust the point value in the script accordingly.

---

## Section 5.3 — First Automated Paper Trade

First, ensure you're in paper mode:

```
Run mcp_alpaca_get_account_info. Confirm the account is a paper account.
If it's live, DO NOT proceed. Tell me immediately.
```

Then run the execution layer in dry-run mode:

```
Read {STRATEGY_FOLDER}/src/execution_layer.py. Add a --dry-run flag.
When --dry-run is set, calculate the order but do NOT call Alpaca.
Instead, print what WOULD have been sent:
- Symbol, side, qty, estimated entry, stop, take-profit
- Account balance before and after (simulated)

Run the dry-run now with a test signal for SPY.
```

If dry-run looks correct:

```
Run the execution layer for real against the paper account.
Use mcp_alpaca_place_stock_order to submit the bracket order.
Verify the order appears in Alpaca's paper dashboard.
```

---

## Section 5.4 — Monitoring and Verification

```
After the first paper trade, use mcp_alpaca_get_orders to list today's orders.
Print:
- Order ID
- Symbol
- Side (buy/sell)
- Qty
- Status (filled, pending, cancelled)
- Filled price (if applicable)

Also read {STRATEGY_FOLDER}/logs/orders.log and confirm the trade was logged correctly.
```
