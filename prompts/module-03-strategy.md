# Module 3 Prompts — Strategy Sourcing (Hermes Edition)

Module 3 is about describing your strategy precisely enough for Hermes to build it.
Hermes uses `delegate_task` to spawn subagents for research, so you can describe the
strategy in plain English and let the subagent extract the structured rules.

---

## How to describe your strategy to Hermes

The more precise your description, the better the code. Use this template as a
starting point:

```
I want to build a trading strategy with the following rules:

Instrument: [e.g. SPY, NQ=F, EURUSD]
Timeframe: [e.g. 15-minute bars, daily bars]

Entry conditions (long):
- [Condition 1 — e.g. 20-period EMA crosses above 50-period EMA]
- [Condition 2 — optional additional filter]

Entry conditions (short):
- [Mirror of long conditions, or leave blank if long-only]

Exit conditions:
- Stop loss: [e.g. 1% of entry price, or $X below entry]
- Take profit: [e.g. 2:1 reward-to-risk ratio]
- [Any additional exit conditions]

Position sizing:
- Risk [X]% of account balance per trade

Any other rules:
- [e.g. only trade during market hours]
- [e.g. no trades on Friday afternoon]
```

---

## The five strategy sources

**Source 1 — Your existing strategy (Section 3.2)**
Describe your manual strategy in the template above. Hermes will translate it into
`rules.json` and Python code.

**Source 2 — GitHub open-source strategies (Section 3.3)**

```
Search GitHub for "trading strategy backtest python" using web_search.
Find a strategy with clear rules. Extract the entry/exit conditions.
Write them into {STRATEGY_FOLDER}/strategies/github-strategy.json in the same format
as our rules.json template.
```

**Source 3 — YouTube transcript mining (Section 3.4)**

```
I want to extract a trading strategy from a YouTube video.
The URL is: [PASTE URL]

Use yt-dlp to download auto-subtitles, convert to text, and extract the entry/exit
conditions. Save the raw transcript to {STRATEGY_FOLDER}/data/transcript.txt and
write the strategy rules to {STRATEGY_FOLDER}/strategies/youtube-strategy.json.
```

**Source 4 — Hedge fund pitch decks (Section 3.5)**

```
Search SEC EDGAR for recent 13F filings or hedge fund strategy descriptions.
Use web_search with query: site:sec.gov "trading strategy" filetype:pdf.
Summarise the logic and write the structured rules to
{STRATEGY_FOLDER}/strategies/hedge-fund-strategy.json.
```

**Source 5 — First principles (Section 3.6)**

```
Start from a market observation. For example: "SPY tends to trend during the
first two hours of the US session and consolidate midday."

Build a strategy that:
- Only trades between 9:30am and 11:30am ET
- Enters long when 20 EMA crosses above 50 EMA in that window
- Exits at 11:30am ET regardless of profit or loss
- Saves rules to {STRATEGY_FOLDER}/strategies/session-strategy.json
```

---

## Checkpoint

By the end of Module 3, you should have a strategy description that answers these
questions precisely:

- What instrument?
- What timeframe?
- What exact conditions trigger entry?
- What closes the trade?
- How is position size calculated?

That description becomes the input to Module 4's backtest.
