# Trading Ideas — captured from phone (2026-09-30)

For: continuing the "Schwab API position sizing and trailing stops" work on laptop.

## Source
- Podcast video: https://youtu.be/pPIPvyticq4 (cloud session could NOT fetch YouTube; transcript not read)
- Likely guest: **Dave Mabe** (davemabe.com): MabeKit + Strategy Cruncher. UNCONFIRMED against the video;
  matches the description (upload backtest CSV with many "columns" -> tool recommends filters).
  Also on Desire To Trade ep. 549: https://www.desiretotrade.com/549-why-i-trade-25-strategies-after-17-years-of-trading-dave-mabe/

## How his system works (from web search snippets; davemabe.com is blocked from cloud, so verify on laptop)
- MabeKit = library of columns/filters (site says 180+; podcast said 250+) you add to each backtest trade.
- Strategy Cruncher = upload backtest CSV (one row per trade + column values + P/L) -> report of
  which column rules remove losing trades "without curve fitting".
- Generic CSV format documented, so any backtester works (Amibroker, Trade-Ideas, RealTest, Python).
- Docs: https://app.davemabe.com/docs, RealTest guide: https://app.davemabe.com/docs/using-real-test
- Cruncher product page: https://app.davemabe.com/buy/optimizer
- Useful posts: /indicators, /the-best-columns-are, /all-columns-arent-equal, /add-multiple-columns-to-your-backtest,
  /what-is-the-csv-format-and-why-is-it-useful, /relative-volume-in-the-premarket, /filter-ideas

## His top 3 columns (podcast) -> start here
1. **ATR** (Average True Range): normalizes moves across stocks (e.g. move / ATR).
2. **RVOL** (Relative Volume): volume so far vs. normal volume *for this time of day*. He calls it maybe the most important filter.
3. **"Range" column**: most likely **Position in Range** (where price sits in its recent N-day high/low range, 0-100%).
   Alternatives he also uses: Opening Range (9:30-10:00 high/low), position lifetime range. CONFIRM from the podcast.
- Other columns he's cited as predictive: Yesterday's Change from Open %, Gap % (start at +/-3.5%).

## Plan
1. Confirm guest + the exact 3rd "range" column (rewatch around the "if I had to pick three" part).
2. On laptop: open davemabe.com docs, get the exact CSV spec + column definitions (the "keys").
   Check whether the column library is public or behind MabeKit login; only use what's accessible under their terms.
3. Build our own versions of ATR, RVOL (time-of-day adjusted), Position in Range in the Schwab project,
   using Schwab price history.
4. Run my own backtest -> export CSV in his format (trade rows + those 3 columns + P/L).
5. Do our own "cruncher-lite" analysis first (bucket each column into quantiles, look at win rate/expectancy
   per bucket, out-of-sample split to avoid curve fitting). Then optionally try the real Strategy Cruncher trial.
6. Fold the winning filters into the existing indicator plan + position sizing / trailing stops.

## Open questions
- Does Strategy Cruncher have a free trial / cost?
- Timeframe: intraday (needs minute bars for RVOL) or daily swing?
