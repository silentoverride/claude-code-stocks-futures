# Strategy Name Prompt

Ask the user to input a name for their trading strategy.

## Instructions

1. Prompt the user: "What would you like to name your trading strategy?"
2. Validate: only lowercase letters, numbers, hyphens, and underscores. No spaces.
3. Create directory at ~/trading/{USER_INPUT}/.
4. Inside it, create:
   - src/         (strategy execution scripts)
   - config/      (strategy-specific settings and parameters)
   - data/        (historical data, backtest results)
   - logs/        (trade and error logs)
   - tests/       (unit tests and validation)
   - strategies/  (JSON strategy definitions)
5. Write a brief strategy-info.md inside ~/trading/{USER_INPUT}/ with:
   - Strategy name
   - Created date
   - Description placeholder

## Example

User inputs: "momentum-breakout"

Resulting structure:

  ~/trading/momentum-breakout/
  ├── strategy-info.md
  ├── src/
  ├── config/
  ├── data/
  ├── logs/
  ├── tests/
  └── strategies/
