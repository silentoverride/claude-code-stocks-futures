# Hermes for Stocks & Futures | Full Course | Build and Earn 2026

> Build a fully automated trading system for Forex, Futures & Stocks using Hermes CLI, kimi-k2.6, Alpaca MCP, and TradingView MCP — no coding experience required.

**Watch the course:** Video URL will be added here after upload

---

## What You'll Build

By the end of this course you will have a live, automated trading system that:

- Pulls real-time and historical price data via Alpaca MCP (`mcp_alpaca_get_stock_bars`) and TradingView MCP (`mcp_tradingview_get_ohlcv`)
- Detects entry signals using your own strategy logic (EMA crossovers, RSI, mean reversion, or any rules you define)
- Sizes positions automatically to a fixed risk percentage per trade
- Places and manages orders on Alpaca without you touching anything (via `mcp_alpaca_place_stock_order`)
- Runs autonomously via Hermes cron jobs (`cronjob`), restarting itself, and sends Telegram alerts when it trades
- Retrains its own parameters using an automated ML loop — you set it running, it improves itself overnight
- Scales to multiple markets and multiple strategies running in parallel as separate cron-scheduled agents

You write no code. You describe what you want in plain English. Hermes delegates to subagents that build it.

---

## Prerequisites

| Tool | Why |
|---|---|
| Hermes CLI | Your agent orchestrator. Runs `delegate_task`, `cronjob`, `memory`. |
| Node.js / Python | Strategy scripts run as Python or Node.js files. |
| kimi-k2.6 | Configured in `~/.hermes/config.yaml`. |
| Alpaca MCP | Already connected (check `mcp_alpaca_get_account_info`). |
| TradingView MCP | Already connected (check `mcp_tradingview_get_ohlcv`). |

If any MCP is missing, see the `tradingview-mcp-setup` skill or add it to `~/.hermes/config.yaml`.

---

## Notion Companion

Full course notes, strategy templates, and resources:
**[Open the Notion companion →](https://www.notion.so/3413d66c134a818c85fbcf140cc5c106)**

---

## Course Structure

| # | Module | Sections | What You Build |
|---|---|---|---|
| 1 | **Setup & Foundation** | 4 | Hermes configured, Alpaca connected, project structured |
| 2 | **Market Data & Signal Generation** | 3 | Live/historical price feed via Alpaca/TradingView MCP, EMA crossover signal engine |
| 3 | **Strategy Sourcing** | 6 | Strategy spec from one of five sources — your edge, precisely defined |
| 4 | **Backtesting & ML Training Loop** | 6 | Scientific backtest baseline, iteration loop, automated parameter sweep |
| 5 | **Paper Trading Execution** | 4 | Full system wired to Alpaca MCP, first automated paper trade |
| 6 | **Going Live** | 3 | Risk management rules, live credentials, first real trade |
| 7 | **Automation & Resilience** | 4 | Hermes cron jobs, Telegram alerts, self-healing error handling |
| 8 | **Expansion & Advanced Moves** | 3 | Multi-market, multi-strategy parallel execution |

**Total: 8 modules — 33 sections**

---

### Module 1 — Setup & Foundation

| Section | Title |
|---|---|
| 1.1 | What We're Building |
| 1.2 | Hermes Config & MCP Check |
| 1.3 | Setting Up the Project Structure |
| 1.4 | Connecting to Alpaca via MCP |

### Module 2 — Market Data & Signal Generation

| Section | Title |
|---|---|
| 2.1 | Connecting to Live Market Data (Alpaca + TradingView MCP) |
| 2.2 | Building the Signal Engine |
| 2.3 | Customising Your Signal |

### Module 3 — Strategy Sourcing

| Section | Title |
|---|---|
| 3.1 | The Fundamental Principle — You Bring the Edge |
| 3.2 | Strategy Source 1 — Your Existing Strategy |
| 3.3 | Strategy Source 2 — GitHub Open Source Strategies |
| 3.4 | Strategy Source 3 — YouTube Transcript Mining |
| 3.5 | Strategy Source 4 — Reverse Engineering Hedge Fund Pitch Decks |
| 3.6 | Strategy Source 5 — First Principles |

### Module 4 — Backtesting & ML Training Loop

| Section | Title |
|---|---|
| 4.1 | The Scientific Method Applied to Trading |
| 4.2 | What a Good Backtest Actually Looks Like |
| 4.3 | Running Your First Backtest With Hermes |
| 4.4 | The Iteration Loop |
| 4.5 | Building the Automated ML Training Loop |
| 4.6 | The Architecture for Long-Running Learning Systems |

### Module 5 — Paper Trading Execution

| Section | Title |
|---|---|
| 5.1 | The Execution Layer |
| 5.2 | Building the Execution Layer (Alpaca MCP) |
| 5.3 | First Automated Paper Trade |
| 5.4 | Monitoring and Verification |

### Module 6 — Going Live

| Section | Title |
|---|---|
| 6.1 | Risk Management Rules |
| 6.2 | Switching to Live Credentials |
| 6.3 | First Live Trade |

### Module 7 — Automation & Resilience

| Section | Title |
|---|---|
| 7.1 | Deployment Options (Hermes cron vs long-running process) |
| 7.2 | Hermes Cron Job Setup |
| 7.3 | Alerts & Monitoring |
| 7.4 | Error Handling & Reconnection Logic |

### Module 8 — Expansion & Advanced Moves

| Section | Title |
|---|---|
| 8.1 | Running Multiple Markets in Parallel |
| 8.2 | Adding a Second Strategy |
| 8.3 | Where Next |

---

## The Prompts

All Hermes prompts used in the course are in the [`prompts/`](./prompts/) folder — one file per module. Copy them directly into Hermes at the relevant point in the course.

---

## Key Hermes Tools Used

| Hermes Tool | Used For |
|---|---|
| `delegate_task` | Spawn a subagent to build/research/backtest |
| `cronjob create` | Schedule the bot to run automatically |
| `memory` | Persist API keys, preferences, risk thresholds |
| `mcp_alpaca_*` | Market data, account info, order placement |
| `mcp_tradingview_get_ohlcv` | Fetch OHLCV bars via TradingView |
| `mcp_tradingview_screen_stocks` | Screen stocks via TradingView |
| `mcp_tradingview_get_indicator` | Technical indicators via TradingView |
| `mcp_tradingview_analyze` | Full technical analysis via TradingView |
| `web_search` / `web_extract` | Strategy research, transcript mining |
| `file` (read_file, write_file, patch) | Read/write strategy code, configs, logs |

---

## Links & Resources

| Resource | URL |
|---|---|
| Alpaca Markets | https://alpaca.markets |
| Alpaca API Docs | https://docs.alpaca.markets |
| alpaca-py Python SDK | https://github.com/alpacahq/alpaca-py |
| Hermes CLI | [Your Hermes install source] |
| TradingView MCP (atilaahmettaner) | https://github.com/atilaahmettaner/tradingview-mcp |
| Apify (YouTube transcripts) | https://apify.com |
| SEC EDGAR (hedge fund decks) | https://www.sec.gov/cgi-bin/browse-edgar |
| yfinance | https://github.com/ranaroussi/yfinance |

---

## About Lewis Jackson

I make videos about AI, crypto, and trading automation.

Everything here is free. No sign-up. No catch.

[YouTube](https://youtube.com/@lewisjackson) | [GitHub](https://github.com/jackson-video-resources)

---

*Trading involves real risk. Everything in this course is what Lewis has tested on his own account — not financial advice. Always use paper trading before risking real money.*
