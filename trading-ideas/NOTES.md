# Trading Ideas — captured from phone (2026-09-30)

For: continuing the "Schwab API position sizing and trailing stops" work on laptop.

## My setup
- Style: **intraday day trading, mostly options contracts**
- Platforms: Schwab thinkorswim (+ Schwab API), TradingView
- Implication: compute columns on the **underlying** (1-min / 5-min bars). RVOL must be time-of-day
  adjusted (cumulative volume vs. avg cumulative volume at the same minute over N days).
  Backtest signals on the underlying first (R-multiples); options P/L needs historical option prices (later).
- thinkorswim: thinkScript studies / scan columns. TradingView: Pine indicators + strategy tester
  "List of trades" CSV export.

## Source (CONFIRMED from transcript)
- SMB Capital "Trading Floor" podcast, ep. 6. Hosts Garrett Dryen, Tim Beldin. https://youtu.be/pPIPvyticq4
- Guest: **Dave Mabe**: systematic day trader since 2005, 30+ automated strategies, ex-CTO Trade Ideas,
  host of "Line Your Own Pockets", founder of **MabeKit** (Strategy Cruncher).
- Free 6-day email course: **betterbacktesting.com** (start here)
- Site/docs: davemabe.com, app.davemabe.com/docs (blocked from cloud; open on laptop/phone)

## His top 3 columns ("desert island" answer)
1. **ATR**: used for (a) position sizing, (b) stop placement, (c) straight filtering.
2. **RVOL**: volume today vs. average (time-of-day adjusted). "Hard to imagine not using it."
3. **Position in Range**: percentile 0-100 (0 = at low, 100 = at high), across multiple timeframes:
   - position in **today's** range
   - position in **yesterday's** range
   - position in **last 10-day** range
   - Also: distance from those levels measured in ATRs; **distance from today's open in ATRs** ("really good").

Other columns he named:
- **Yesterday's range** (strongly predictive in many setups)
- Gap % (his gap strategy started at 3%, optimal came out ~3.8%)
- Avg volume, ATR, ATR^2 / price as **universe filters** (hosts' idea)
- Yesterday's change from open % (from his blog)

## His process (the "keys")
1. **Signal matters least.** Everyone trades the same setups; good traders *skip the bad trades*.
   Old setups (e.g. opening range breakouts) still have edge with the right filters.
2. **First backtest = wide net.** Loosen thresholds (e.g. gap 3% instead of your 5% guess) -> many more trades.
   A down-sloping P/L curve on the first test is fine; the winners are in there.
3. **Universe filters vs. predictive filters.** Universe = fence in tradable names (min avg volume,
   min gap, etc.). Predictive = the rules that create edge. Don't make the net so wide trades aren't comparable.
4. **Column library.** Every backtest includes the same big shared set of columns (he has 250-300,
   one trader has 400+). Add new columns when you notice something; back-apply to old strategies.
5. **Optimizer (Strategy Cruncher).** Upload CSV -> it finds the optimal cutoff per column, **ranks** columns
   by improvement, shows **one rule at a time** so you think about each one. Use exact optimal values (3.8 not 4).
   Fewer, stronger rules = less curve fitting. Big trade count = main defense against curve fitting.
6. **Check the charts** for the top-ranked columns: understand *why*; this often spawns new strategies.
7. **Exits:** backtest with **no stop/target, time stop only** -> use **MFE/MAE** to pick the optimal stop ->
   rerun with that stop -> then optimize filters. (Advanced: compare optimization with vs. without stop.)
   - **Partial profits (half off + stop to breakeven) = one of the worst things you can do** per his math.
   - Targets depend on the strategy: short-hold/small-move -> fixed target (e.g. 1R, Adam Grimes story) can win;
     trend-all-day -> no target, exit at close.
8. **Idea generation:**
   - Daily review of all trades + unusual market moves.
   - "Why didn't my model take TSLA today?" -> maybe a missing column.
   - Near-misses just past your thresholds often hold edge.
   - **Worst losers -> trade the opposite direction** as a separate strategy (his gap-long -> fade strategy).
   - Strong vs. deliberately weak signal comparison.
   - "Gun to your head, trade the opposite side": what would you need to see?

## Plan
1. Sign up for betterbacktesting.com; on laptop read app.davemabe.com/docs for the CSV spec + column definitions.
2. Build our own column library, starting with ATR, RVOL (time-of-day), Position in Range
   (today / yesterday / 10-day), distance from open in ATRs, yesterday's range, gap %.
   Use Schwab API price history (minute bars).
3. Pick one base setup (e.g. opening range breakout on the underlying), wide-net backtest, time stop only.
4. MFE/MAE -> optimal stop -> rerun.
5. "Cruncher-lite": for each column find the best single cutoff, rank by improvement, apply one rule at a time,
   validate out-of-sample. Then optionally try the real Strategy Cruncher.
6. Map winning rules to options entries + ATR-based sizing/trailing stops (existing Schwab work).
   Skip partial profits unless the data says otherwise.

## Open questions
- Strategy Cruncher price / trial?
- Historical intraday option prices source for realistic P/L?
- Which base setup to start with?
