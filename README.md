# For-Release

This repository contains app releases while the project's private implementation
details remain separate.

# Pensive Trader

A Tkinter-based stock screener for searching ticker symbols, tracking price
changes, opening detail windows, and viewing market charts. It also includes a
FastAPI web dashboard for responsive quote and chart views.

## Live Website

Visit the live project: [Pensive Trader](https://pensivetrader.com/)

## Current Changes

- Added "Compare with SPY" to the Trade Journal. Closed trades in the current view
  can be grouped by setup or by a new optional "Idea source" tag (Own idea,
  Community / trade room, Other). For each group, return on starting capital is
  compared with holding SPY over the same dates (entry-day open to exit-day close,
  dividends excluded), alongside SPY's average move while trades were open.
  Groups under 30 trades are flagged as small samples. Only the date range is sent
  to the server; trades stay in the browser.

- Desktop Chart pages expand up to 1600px wide, with a viewport-aware canvas
  up to 800px tall while retaining the centered, stacked title and controls.
  Analysis releases its
  narrow page-width limit while Chart is selected; other ticker pages retain
  their compact layout. Mobile sizing is unchanged.

- Improved the main ticker chart's layout: compact controls, a larger mobile
  plot, and consistent canvas/display dimensions so card padding and competing
  height rules no longer stretch or compress the rendered chart. Chart data,
  view selection and interactions are unchanged.

- Refined the shared ticker-page modal with a compact, light navigation header,
  aligned 44px arrow/select/close controls, clearer focus and disabled states,
  consistent spacing and softer card borders. Quote summaries use three columns
  on desktop and two on mobile.
  Moving-average context and supporting text are more compact. Styling is scoped
  to the shared ticker-page layout so standalone views retain their appearance;
  navigation, calculation and request behavior are unchanged.

- Watchlist and Screener ticker details use one primary modal; Analysis search
  results use the same layout inline below the search box, without a popup or X.
  Duplicate page action buttons are removed from view in favor of the dropdown
  and arrows. Learn More, Fundamentals,
  Chart, Options, score history and trade assessment open
  as internal pages. Previous/next arrows follow a fixed page order, with a
  page selector between them and stock-only pages omitted for ETFs. The redundant
  header title is removed. Escape on a child page returns to its parent; the
  single X closes the overall Watchlist/Screener modal. Analysis keeps its quote
  and summary on the Details page of the inline result.
  Show Breakdown is restored on Details and opens a separate modal; Trend
  Analysis opens a separate modal above Chart. These are the only analysis
  actions that launch additional modals; Options breakdown remains an internal
  page. Closing either modal retains the underlying page and its controls.
  Existing renderers, requests, caches,
  form controls and chart interactions are retained; closing a page runs its
  existing cleanup. Other standalone dialogs are unchanged.

- Trend Analysis quote price, dollar change and percentage share the percentage
  direction color: green positive, red negative, black zero. Missing values
  remain N/A; unavailable percentage direction does not imply a color.

- Trend Analysis shows only interactive confirmed-pivot dots, not added close
  markers. Mobile context cards use compact rows. A separate provider-quote
  snapshot row shows current price, dollar change and percent change versus
  previous close; it is distinct from the completed-bar chart endpoint.
  Selector/action alignment uses matching heights and removes inherited label
  margins; mobile context rows omit secondary notes (retained in evidence).
- Trend Analysis dots support hover, tap, and keyboard inspection of prices,
  displacement from open, timestamps and pivot confirmation times. Axis and
  detail timestamps use the viewer's local timezone with timezone labels;
  session calculations remain anchored to the U.S. regular-session open.

- Trend Analysis uses responsive SVG coordinates on mobile, preserving chart
  height and axis-label readability instead of shrinking a desktop plot.
  The plot redraws when its container resizes; the open anchor is unchanged.

- Trend Analysis uses a white modal and the shared X close-control styling,
  with accessible close labeling. Light cards and high-contrast chart colors
  replace the separate dark theme; opening-price normalization is unchanged.

- Renamed the shared chart action to Trend Analysis, placed beside the chart-view
  selector. Its visual context modal uses the shared centered analysis-dialog
  positioning, sizing limits, border, shadow and scrolling rules.

- Chart analysis is now visual-first: compact context cards, a completed-close
  displacement chart with a dominant zero/open line, candidate zone bands and
  confirmed pivot markers, a close-proportion bar, and compact level cards.
  Detailed evidence and calculation methods are collapsed by default.
- Added on-demand Analyze Chart context to shared web charts, including the
  focused chart. The modal requests a five-minute snapshot without changing
  chart mode, opening-price normalization, or existing scores. Only verified
  completed provider bars are analyzed; missing completion/open data is explicit.
  A lone closing print right-labeled 16:01 after the completed 16:00 bar is
  explicitly excluded from analysis, not treated as an extra five-minute bar.
  Position versus open, proportions of completed closes on each side, crossings,
  and recent direction are separate observations. Recent direction needs six
  consecutive bars and uses median pairwise slope; the neutral band is the greater
  of one cent or 0.02% of open. Persistence labels require 75% of available closes.
  Candidate support/resistance zones use strict pivots with two completed bars
  on each side (ten-minute confirmation delay); gaps are never bridged for pivots
  or recent direction. Zones use OHLC extrema when all bars have validated OHLC,
  otherwise explicitly use completed closes. Pivot-price clusters span at most
  twice their padding (the greater of one cent or 25% of mean bar range / mean
  absolute close change), and show up to three nearest zones per role. Roles are
  relative to the latest completed close, not predictive confidence. The card
  discloses timestamps, coverage, methods, and its snapshot-only nature; reopen
  to refresh. Automated trendlines/formations are intentionally deferred.

- Intraday scores now use completed bars only. The latest still-forming bar is
  set aside (chart unchanged), so 5m, 15m, 30m and 1h readings are ready
  instead of provisional for most of each bar. The score updates when each bar
  closes. Price path, matched-bar alpha, VWAP window and relative volume all
  stop at the same completed bar. A timeframe needs two completed bars plus
  the 9:30 open anchor (15m from 10:00, 30m from 10:30, 1h from 11:30).
- Relative-alpha differentials now remove the part of a move explained by the
  ticker's usual sensitivity to SPY and its sector benchmark
  (`ticker - beta x benchmark`), using a robust beta estimated once per day
  from the prior 10-20 complete five-minute sessions only. Falls back to the
  raw difference when history is too thin; long-term intervals are unchanged.
  The breakdown notes when the adjustment was applied, and Model Insight
  reconstruction uses the same prior-only estimate.
- Added two more optional descriptive rows: the latest five-minute move versus
  the usual move at the same time of day, and range-based (high-low) variation
  so far versus the same time in prior sessions. Neither affects the score.
- Added collapsed, optional descriptive context in the score breakdown:
  move versus recent variation, path efficiency, approximate same-time volume
  percentile, robust recent slope, and an OHLC typical-price VWAP cross-check.
  These are secondary observations only; they do not affect the score.
- Fixed historical score reconstruction to build relative-volume profiles from
  only the prior 10-20 completed ticker sessions. The session being scored and
  later sessions are excluded; relative volume remains unavailable until there
  are at least 10 prior complete sessions.
- The score breakdown has its own intraday timeframe selector (1/5/15/30/60
  minutes) and selected-timeframe score. Overall Summary remains the usable
  multi-timeframe average; the breakdown does not present an averaged factor model.
- Overall Summary is now the main web analysis score display: removed the analysis
  timeframe selector and separate Bullish/Bearish Score card. Average, timeframe
  scores and spread use two decimal places; the scale explanation sits immediately
  below the average. The breakdown remains the default one-minute observation.
- Centered the Overall Summary direction-count/spread readout. Corrected final
  shortened intraday bars remaining provisional after the source reaches session
  close; truncated or missing-minute partial bars remain provisional.
- Removed public-facing internal composite and declared/effective weight displays.
  Overall Summary now defaults to labeled timeframe graphics, coverage/readiness
  and a concise spread/count readout; longer context is expandable. The web factor
  breakdown uses signed score-point bars, with missing evidence explicitly N/A.
  These are decision-support observations, not buy/short prompts. Calculation/API
  weights and qualification behavior are unchanged.
- Added descriptive timeframe disagreement alongside existing averages: individual
  readings/readiness, score range, population SD and >=65/<=35/neutral counts.
  Web analysis summaries, score tooltips and retained candidate/history evidence
  distinguish mixed from aligned readings without changing strict qualification.
  Added all-factor signed score-point contributions
  to web/desktop breakdowns and candidate observation context. Missing factors
  remain explicitly unavailable; historical contribution accounting shares the
  calculator's decomposition. Weights, scores, thresholds and provider calls are
  unchanged. Neither disclosure is predictive confidence.
- Refined the descriptive VWAP support factor from binary +/-1 to
  `tanh((price - VWAP) / (2 * volume-weighted session price dispersion))`.
  It is neutral at VWAP, symmetric, smooth and bounded, with unchanged weights,
  directional thresholds and strict Above/Below filters. Flat/insufficient
  dispersion is explicitly unavailable; raw VWAP remains usable for filters.
  Live analysis, plotter scoring and historical Model Insight/similar-setup
  observations share the formula. Historical source granularity is disclosed.
  Web, desktop and candidate context show dispersion-normalized distance and
  additive score-point contribution as support/extension, never prediction or
  entry quality. No extra provider requests are needed.
- Added Candidate Review with session-only, browser-isolated screening
  criteria, evidence, lifecycle states and up to the latest 100 evaluations per stock.
  Market Screener retains its existing one-time results or can start a continuous
  review and opens Candidate Review automatically. There is no separate one-time
  Candidate Review workflow or duplicate review button; existing reviews remain
  accessible from the main navigation. Guidance lives in the collapsed
  screener guide. Reviews never automatically change the
  personal watchlist; explicit Pin is available. Full analysis opened from a
  candidate opts out of the Analysis page's legacy automatic watchlist tracking.
- Candidate Review follows the shared page background, spacing and dark card
  styling. Its guide clarifies the 500-stock discovery limit and
  that repeated qualification is filter consistency, not a best-stock ranking.
- Candidate Review bounds backend retention to 5,000 latest candidate statuses
  and 5,000 history entries across all reviews. Oldest history is trimmed with
  visible disclosure; candidate-capacity exhaustion explicitly pauses the review
  rather than dropping candidates silently. Latest status and history share one
  stored snapshot, and evidence JSON is encoded off the server event loop.
  Browser requests time out after 10 seconds and subsequent polls recover without
  a page reload, retaining potentially outdated evidence. Non-JSON proxy responses
  and malformed JSON report the HTTP status instead of a misleading parser error.
  Saved filters do not
  change when the screener controls are edited later.
- Continuous reviews are selective: a first complete pass adds a candidate;
  stocks that have never matched are not retained as potential candidates.
  Two consecutive measured failures remove the candidate and its review history.
  Unknown data is disclosed for existing candidates without becoming a measured
  failure. Daily filters run before intraday analysis; selected intraday EMA,
  VWAP, RVOL and freshness checks run on the one-minute reading before remaining
  timeframes. Failed/unavailable checks skip the remaining analysis that scan.
  Directional scoring stops when even the remaining readings cannot meet the
  selected threshold; without a score filter, three usable readings are enough.
  Background scans process two stocks at a time. The default retained-candidate
  view discloses Weakening/Data unavailable states until removal or recovery.
- Continuous reviews run only during today's regular NYSE session, beginning
  after the first five-minute bar plus a 15-second provider allowance. The exchange
  calendar respects holidays, daylight saving and early closes. Reviews expire
  at that day's close and do not resume next day. A complete pass qualifies,
  and two consecutive measured failures remove; unknown/stale quotes or one-minute bars
  interrupt streaks without becoming failures.
  Pausing is terminal for scheduling in this release.
- Candidate evidence reuses existing price/market-cap/average-volume, percentage,
  intraday score/EMA/VWAP, daily EMA/SMA and RVOL filters. At least 3/5 usable
  intraday scores and quotes/one-minute bars within 120 seconds are required.
  Stored profiles include available one-minute regime, sector-relative returns,
  normalized factor scores, and VWAP distance without introducing new weights.
  Provisional coverage and daily dates remain disclosed. Entry, spread/depth,
  events, borrow and costs are not vetted; qualification is not a trading verdict.
- Monitoring requires a running backend, not an open browser. Candidate criteria,
  profiles and evaluation history live only in backend memory and are discarded
  at today's session close (including completed/paused reviews) or backend restart.
  There is no candidate database, disk storage or environment-variable setup.
  Deploy with one backend worker/instance; state is not shared between processes
  or replicas. Hosts that sleep/scale to zero interrupt monitoring and lose state.
  A private HTTP-only browser cookie owns reviews; clearing cookies loses access,
  and there is no account/cross-device recovery in this release.
  Limits are 3 pending/active monitors and 10 session reviews per browser, 20 active
  monitors and 100 total session reviews per backend process, and 500 eligible
  stocks per scan (larger searches require refinement, never silent truncation).
  History retains up to 100 evaluations per candidate within the shared budget
  and is discarded at the close.
  In-memory leases prevent overlapping evaluations of a monitor and recover
  interrupted runs after at most 180 seconds without a heartbeat. Missed intervals
  are not replayed; provider speed/server load may delay scans. At close, no new
  evaluations are scheduled and incomplete results are discarded; already-issued
  synchronous provider requests may finish in their worker threads.

- Simplified Market Screener score cells to the percentage alone. Coverage,
  provisional status, quote age, relative volume, and daily-data details are
  retained in hover tooltips and accessible labels; score rules are unchanged.

- Market Screener Score filtering/ranking and Statistical Analysis's overall
  direction/average now require at least 3 of 5 usable intraday timeframes,
  rather than waiting for every longer timeframe. Averages use only valid
  readings, disclose coverage, and are provisional when coverage is reduced
  or bars are developing/unknown. Fewer than three readings do not qualify.
  Individual timeframe data-readiness checks and the stricter Assess Trade
  Setup five-timeframe requirement remain unchanged.

- Statistical Analysis now suppresses directional scores and regimes when
  fewer than three valid chronological intraday observations are available
  (two for historical views). Overall summaries exclude insufficient readings,
  report timeframe coverage, and disclose included provisional readings.
  Developing-bar metadata marks individual regimes/scores as provisional;
  missing completion metadata is explicitly unknown, not assumed complete.

- Moved the Market Screener explainer into a collapsed, keyboard-accessible
  "i" guide beside the page heading, keeping all filter explanations available.

- Replaced Market Screener presets with Percentage and Direction only types.
  Percentage retains comparison operators and optional direction-score filtering;
  Direction only ignores the move from the previous close and uses the same
  bullish/neutral/bearish score thresholds with at least 3/5 usable timeframes. Optional
  confirmation filters remain available in both modes.
- Retained price, market-cap, and three-month average-volume eligibility gates
  and added adjustable minimums. Scans accept up to 500 supported candidates;
  larger searches ask users to refine those minimums instead of silently
  truncating results or starting scoring. Direction candidates use average-volume
  ordering, never percent-change ordering. Worker concurrency is unchanged.

- Simplified the assessment's main checklist to RVOL, VWAP, 5-minute 9/21
  EMA, and complete bullish/bearish score for day trades; daily 9/21 EMA
  and 50/200 SMA for swings. Values/thresholds expand on demand, unmet data
  or entry checks retain a brief summary without a Warnings section, and risk results live inside the optional
  collapsed plan. The close button now matches the other modals' 32px control.

- Streamlined assessment results into compact Pass/Fail/Unknown rows with
  expandable rule explanations and status totals. Removed the Manual checks
  section in favor of one short limitations notice, and positioned the close
  control at the top-right. Measured thresholds and risk calculations are unchanged.

- Fixed analysis HTTP 500 errors when daily trend enrichment loads longer
  history: assessment bar-date metadata now uses the collector's supported
  1-Year-Daily snapshot rather than a nonexistent All-Time-Daily interval.
  Added a real-collector regression test and moved Assess Trade Setup beside
  Review Score History on Analysis, Screener, and Watchlist.

- Added a shared Assess Trade Setup button on the Analysis page and stock
  detail modals in Screener and Watchlist. Day/Long, Day/Short, Swing/Long,
  and Swing/Short checklists show Pass/Fail/Unknown with actual values and
  visible heuristic thresholds; there is no buy/sell verdict or profit probability.
- Day rules require the regular session, all five valid intraday readings,
  quotes and the latest one-minute bar within 120 seconds, directional average
  at least 65 (long) or at most 35 (short), matching 5-minute EMA/VWAP direction,
  relative volume at least 1.5x, and price within 1% of VWAP. Swing rules use
  at least 200 daily observations, a disclosed daily bar date, matching daily
  EMA/SMA direction, and price within 5% of the daily 9 EMA, never an intraday
  score relabeled as swing. Both require price > $5 and average daily volume
  > 1 million shares; daily freshness/completed-bar structure needs manual review.
- Optional, unsaved stock entry/stop/target plans check price ordering,
  reward/risk at least 2:1, and entry proximity. Dollar risk budget produces
  whole-share sizing, stop-distance risk, and entry notional before costs/gaps.
  Spread/depth, events, and support/resistance remain explicitly Unknown;
  passing measured criteria never means all execution/entry checks are complete.
  Results are snapshots with manual refresh; open day assessments re-evaluate
  freshness every 15 seconds. Requests are cancelled on close or stock change.

- Consolidated Trade Journal explanations into one top-level, keyboard-accessible
  "i" guide with an annotated, fictional sample journal that never changes saved data.
  Kept the introduction and browser-storage notice visible, aligned the starting
  capital input and save button, and spaced Reset view & filters from statistics.
  Status messages, recovery warnings, and deletion confirmations remain visible.

- Improved the Trade Journal with optional setup and day-trade/swing tags,
  original planned dollar risk, realized R-multiples, and exact-symbol,
  setup, and style filters that combine with the existing date views.
  Older records/backups remain compatible; absent tags/risk stay unknown.
- Added average net win/loss, historical net expectancy, profit factor,
  average R with sample counts, and a cumulative daily realized P&L chart
  with accessible data table and daily peak-to-trough drawdown. Open trades
  and plans are excluded; undated exits are excluded only from the chart
  and drawdown. Starting capital plus all closed P&L remains unfiltered.
  Saved risk corrections require confirmation; the cash-flow P&L math
  and browser-local storage/import safeguards are unchanged.

- Market Screener scores require at least three valid intraday readings.
  Results disclose coverage and provisional status; fewer than three readings
  display an informational partial average, excluded from filtering/ranking. Regular-session
  quote age uses the provider timestamp rather than calculation time; unknown
  source timestamps remain explicitly unknown. Watchlist/assistant averages
  retain their existing available-reading semantics.
- Removed the Market Screener's fixed 1-million-share accumulated-volume
  minimum for today, retaining its price, market-cap, and average-volume
  eligibility gates. Added optional time-adjusted relative-volume filtering
  with the existing historical-profile/linear-fallback source labeling.
- Added bullish/bearish Day and Swing filter presets. Day uses the complete
  intraday score, intraday EMA, and VWAP; Swing uses daily EMA and SMA and does
  not relabel the intraday score as a swing score.
- Daily EMA/SMA filters use a bounded daily-only enrichment endpoint, reuse
  existing moving-average formulas, and retain results within the current
  scan. Selecting a daily filter no longer restarts intraday scoring.
  Daily bar dates and retrieval failures are surfaced explicitly.
- Decoupled prior-session intraday EMA seeding from daily SMA history so
  early-session EMA confirmation remains available without selecting SMA.
  Seeding does not change the directional score's factor weights or value.
- Limited displayed market candidates to the same 200-symbol scoring limit
  and explicitly reported when additional supported matches are omitted.

- Added a daily 9/21 EMA ("swing_trading") trend indicator alongside the
  existing 5-minute 9/21 EMA ("day_trading"), surfaced on the Analysis page,
  Watchlist, Market Screener, and desktop analysis window.
- Fixed the 5-minute 9/21 EMA ("day_trading") being unavailable for roughly
  the first 105 minutes of every trading session: it previously only had
  that day's bars to work with (needing 21 of them before the EMA could be
  computed at all), resetting from scratch every morning. It's now seeded
  with several prior sessions' 5-minute bars, so it's available immediately
  at the open and carries over across sessions like a real EMA.
- Significantly sped up Market Screener scoring (roughly 6x faster on typical
  scans): the 50/200 SMA filter's extra price-history fetch now only runs
  when that filter is actually selected (now selectively enriching daily
  indicators if picked after the scan starts), screening concurrency was
  raised from 4 to 10 since the work is network-latency-bound rather than
  CPU-bound, and Yahoo/Nasdaq requests now reuse one shared HTTP connection
  pool instead of opening a new one per symbol.
- Added 9/21 EMA, 50/200 SMA, and Price vs VWAP filters to the Market
  Screener, narrowing results to candidates whose trend indicators agree
  with the Score filter.
- Renamed the Market Screener's Direction filter to Score, with explicit
  thresholds shown in each option (Bullish >=65, Bearish <=35, Neutral 35-65).
- Fixed the Market Screener's filter controls packing into an uneven grid at
  common screen widths; they now lay out in clean, evenly filled rows with
  the Screen Stocks button centered below, and sized to its own text instead
  of stretching full width on mobile.
- Fixed Year-to-Date/All-Time/N-Year statistical-analysis scores reading as
  more confident than their evidence supports. Acceleration, Relative
  Volume, and VWAP Position confirmations are structurally unavailable on
  these intervals (they require same-day intraday bars); the composite
  weight no longer renormalizes over only the remaining factors, so their
  absence now dilutes the score toward neutral instead of inflating the
  other three factors by roughly 1.5x. Intraday renormalization, where
  these gaps are usually transient, is unchanged.
- Limited the Analysis modal's five-timeframe cross-check summary to
  intraday intervals; Year-to-Date/All-Time/N-Year views now summarize only
  their own timeframe instead of fetching and averaging unrelated
  1/5/15/30/60-minute readings.
- Added hover tooltips to every Market Environment factor card (and the Live
  Fixed-Income Confirmation card) explaining what the underlying FRED/Yahoo
  series measures and why it matters.
- Fixed the header navigation links wrapping mid-word at common desktop
  widths; the nav now wraps whole links onto a new row instead of breaking
  inside a link's text.
- Added optional Discipline & Mindset fields to the Trade Journal (emotional
  state at entry, whether you followed your own rules, and a reflection
  note) plus a win-rate-when-rules-followed-vs-deviated statistic. None of
  these affect P&L or win rate.
- Added a Trade Journal section to the About page.
- Added an options analyzer. It builds a strategy around a target delta from
  the Nasdaq chain (calls, puts, covered calls, cash-secured puts, vertical
  spreads, straddles, strangles, iron condors) and scores probability of
  profit, breakevens, max profit/loss, and expected profit against the
  underlying's realized volatility, with a strike ladder from deep in the
  money to far out of it and early-assignment warnings for short legs.
  - Positions can be held to expiration, closed on a chosen date (any
    contract with 2+ days to expiry), or closed intraday after a set number of
    trading hours, including same-day (0DTE) contracts. Results show the move
    needed by exit and the time decay paid while holding.
  - Time to expiry is measured to the 4 p.m. ET close and projections count
    actual trading sessions, so short-dated contracts are priced correctly.
  - Intraday holds use a time-of-day volatility profile built from ~45 days
    of Yahoo 5-minute bars (overnight gaps excluded, cached per symbol per
    day), falling back to daily-close volatility if the bars are unavailable.
  - Figures use mid prices and hold implied volatility constant; analyses run
    after hours use the last closing quotes.
- Added an **Options** button beside **Fundamentals** on the Analysis page.
- Web Screener and Watchlist rows now open one combined dialog with the chart,
  statistical analysis, and options analysis; the per-row Analyze and Options
  buttons were removed.
- Fixed the options dialog close control rendering at the bottom-left on
  mobile; it now matches the other dialogs' fixed top-right button.
- Web ticker logos now load from Parqet first, falling back to Financial
  Modeling Prep, whose image host had been timing out.
- Replaced the web Market Screener's Bias and Agreement filters with an
  Average score filter. It offers the same Greater than, At least, Less than,
  and At most operators as the percent-change filter, applied to an optional
  0-100 Score % threshold. The Bias column was removed from screener results;
  the Watchlist's Overall Intraday Screen keeps its bias and agreement filters.
- Reworked the web Analysis page into three stacked sections: price and quote
  summary, graph, and statistical analysis. The ticker action is now
  **Analyze**; analysis and chart content no longer open in modals on this page.
- Restored stock-only company research. **Learn More** opens a dedicated,
  lazy-loaded Wikipedia/Wikidata modal after a stock is analyzed, and the
  Pensive bot uses the same company profile source. ETFs are excluded from
  this research path while retaining normal quote and analysis support.
- Added VWAP position to the per-ticker score. Yahoo quote summary has no VWAP
  field, so the app derives a session approximation from Yahoo one-minute
  closes and bar volumes. Its support score now uses smooth session-dispersion
  normalization rather than a binary sign; the declared composite weight is `0.15`.
- Updated Year-to-Date behavior so the latest point and return use current
  price during regular trading. The post-close historical refresh replaces
  the provisional point with Nasdaq's official daily close, and provisional
  values are not written to the persistent YTD cache. Date-only YTD labels now
  use an explicit UTC contract so they no longer render one day behind.
- Fixed pre-market fallback charts to anchor their timestamps at 9:30 ET and
  use previous close when today's regular-market open is unavailable.

## Features

- Search up to 5 ticker symbols at a time.
- View live price, change, and percentage change information.
- Open individual ticker detail windows.
- View chart options for different time ranges.
  - Pre-market views show only pre-market values.
  frequency, factor influence, and data coverage without claiming predictive

- Added All Time chart views at weekly, monthly, and yearly resolution to the
  daily histories concurrently, select Nasdaq unless Yahoo returns more usable
- Added chart navigation on every surface: web Canvas charts support
  wheel/trackpad and two-finger-pinch zoom, horizontal drag pan, and Reset
  and reset toolbar.
- Fixed stale Year-to-Date series across an Eastern calendar-day change by
  to an earlier date.
- Added a non-predictive **SPY Intraday Regime** to every desktop and web
  persistence confidence, and optional live HYG/IEF/LQD/TLT/SHY confirmation;
  it is the same context readout whether the selected symbol is SPY, a stock,
- Added confirmed acceleration as a low-weight statistical-analysis factor.
  samples, so missing confirmation is shown as unavailable rather than neutral.
  to the SPY Intraday Regime or per-ticker statistical model.
- Added a portrait-mobile hamburger dropdown for secondary navigation. The
  phone landscape keeps the original visible navigation.
- Added `/model-insight`, a dedicated web page for the Model Insight
  report audits model behavior rather than forecasting returns, and renders the
  API response as readable summary, interval, regime, factor, and coverage
- Added `GET /api/model-insight`, backed by
  and summarize Bullish/Bearish Score distributions, composite-weight
  contributions, and source coverage.
- Added Yahoo historical intraday bar support for calibration and reused the
  is visible instead of silently neutral.
  to **Bullish/Bearish Score**. Compatibility aliases remain in the API for
- Improved the per-ticker analysis model with interval-matched Relative Alpha
  and historical intraday relative-volume confirmation. Missing benchmark or
  volume data is reported through source/coverage fields instead of being
  treated as neutral evidence.
## Version 0.0.7


- Added a FastAPI web dashboard alongside the Tkinter desktop application.
- Added asynchronous JSON refreshes for quote data and chart interval changes.
- Added shared navigation and responsive styling across the HTML pages.
- Added desktop chart logos and hover-based price inspection.
  as the price anchor.
- Matched website chart axis intervals to the desktop chart presentation.
- Restored Financial Modeling Prep logo URLs across web and desktop charts.
- Added bundled splash-screen and background image resources for PyInstaller
- Updated the PyInstaller spec and build helper to include image resources.
- Added a persistent watchlist (backed by `localStorage`) shared between the
  ticker logos and price-direction coloring on every row.
- Restricted website charts to regular trading hours (9:30 AM - 4:00 PM ET),
  matching the "current session only" behavior across every chart surface.
  open/close wiring, chart logos, price-direction coloring) into a shared
- Split the monolithic `pensive_trader.css` into focused stylesheets
  slim base file, so each concern can be maintained independently.
- Standardized action-button sizing on mobile via a shared `.action-button`
  Chart, View Watchlist, Clear All, etc.) has identical dimensions.
- Removed the former standalone Wikipedia/Wikidata Company Search page. The
  current release restores stock-only research through the Learn More modal
  and Pensive bot instead of reviving that separate page.
- Added a Yahoo Finance Market Screener page and persistent desktop screener
  window with matching Filter, Percent change, and Screen Stocks controls.
  and clickable sorting for both web and desktop result tables.
### 2026-09-21
- Refactor: centralize fetcher creation (`common/fetchers.py`) and prefer Nasdaq minute bars as the canonical source; Yahoo used as fallback.
- Add `common/chart_processor.py` with minute-frame builders and a modulo-based aggregator (`aggregate_from_minute_df`).
- Move normalization helpers to `common/normalizers.py`.
- Fix timestamp encoding for aggregated bars (treat Eastern wall-clock as UTC to preserve provider epoch-ms), and add unit test `common/Unit Test/test_chart_aggregator.py`.
- Added an Analyze Selected action to the desktop Market Screener. Quote
  history is prepared on a worker thread before the statistical-analysis
  window opens.
- Converted the desktop Watchlist from stacked labels into a sortable table
  matching the Market Screener. Rows include logos, ticker and company names,
  price, change, change percentage, and active extended-hours data.
- Increased desktop Watchlist row height to 34 pixels so logos and quote
  values have clearer vertical separation without changing the denser
  Market Screener and Sectors tables.
- Added Analyze Selected and Remove Selected actions to the desktop Watchlist;
  double-clicking a loaded row opens its ticker detail window.
- Added Show Screener/Hide Screener desktop visibility text beside the
  watchlist toggle.
- Added a normalized statistical analysis model based on relative alpha,
  integral trend persistence, and derivative velocity. The result exposes a
  composite weight, Bullish/Bearish Score, factor scores, and market regime.
- Added `/api/analysis/{ticker}` so desktop and web interfaces use one
  statistical-analysis result format.
- Added statistical-analysis dialogs to the web Analysis, Screener, and
  Watchlist workflows, with an X-only close control in the top-right corner.
- Expanded the shared browser analysis modal to match the desktop analysis
  hierarchy with price/change/timeframe context, SPY and sector-ETF excess
  returns.
  Mobile screens use stacked cards, a persistent close control, and vertical
  scrolling without horizontal overflow.
- Added a smooth color, background, border, and highlight transition when the
  shared analysis modal enters or changes its bullish, bearish, or neutral
  Market Regime state. Reduced-motion preferences disable the animation.
- Restored fixed viewport centering and clearly rounded 16-pixel corners for
  the shared analysis modal. Mobile layouts retain a consistent 12-pixel
  viewport inset while expanded analysis content scrolls inside the modal.
- Centered the Analysis page's returned quote data beneath the ticker search
  controls. The quote panel matches the search form's 560-pixel desktop width,
  centers every summary row, and contracts to the available content width on
  mobile screens.
- Added the stock sector to every desktop analysis window and browser analysis
  modal. Yahoo asset-profile data supplies the sector when Nasdaq omits it.
- Aligned web and desktop Bullish/Bearish Score inputs by making the analysis API load
  current SPY and mapped sector-ETF returns. Provider sector aliases such as
  Healthcare, Financial Services, and Basic Materials resolve to the same ETFs
  used by the desktop analyzers.
- Expanded spacing inside desktop analysis cards and removed their redundant
  in-window X while retaining the standard title-bar close control.
- Matched the desktop Market Regime badge to the website's bullish green,
  bearish red, and neutral amber foreground, background, and border colors.
- Expanded the shared desktop analysis window with separate score cards,
  one-minute timeframe context, SPY and sector-ETF excess returns, normalized
  factor scores, raw integral and average-derivative values, and the composite
  factor weighting mix.
- Added an Analyze action to every web Screener result row and every Watchlist
  ticker row.
- Added a batched Watchlist quote endpoint so all saved ticker rows begin
  loading together instead of waiting for individual refresh turns.
- Added a centered Sectors Watchlist button beneath the web Watchlist search
  bar. It opens a centered modal with current quotes for every configured
  sector ETF.
- Added aligned sector-modal columns for logo, ETF symbol and sector name,
  price, change, and change percentage. Logos use a small safe inset so they
  remain fully visible at the list edge.
- Added a Show Sectors/Hide Sectors desktop control that opens a dedicated
  sector-ETF `Toplevel` window while retaining the always-visible SPY row in
  the main window. The window presents live sector values in a sortable table
  with logos, symbols, sector names, prices, changes, and percentages.
- Routed SPY and configured sector ETF analyzer updates through the dedicated
  market-analyzer queue so they cannot be mixed with ad-hoc ticker results.

### Statistical Analysis

Version 0.0.5 introduced a bounded statistical weighting model that now
combines six views of a ticker's current behavior:

| Factor | Input | Purpose
| --- | --- | --- |
| Relative alpha | Interval-matched return differences versus SPY and the mapped sector ETF | Measures market and peer outperformance on the selected timeframe
| Integral persistence | Average normalized area represented by `current_integral` | Measures whether recent direction is sustained without time-of-day saturation
| Derivative velocity | Spline slope represented by `avg_derivative` | Measures immediate directional momentum
| Acceleration confirmation | Two material, same-direction completed-bar spline second-derivative samples | Confirms that velocity is consistently increasing or decreasing
| Relative volume | Current cumulative volume versus expected historical minute-of-day cumulative volume | Confirms whether participation supports the existing price direction
| VWAP position | Current price versus a VWAP approximation from Yahoo one-minute closes and volumes | Smooth signed session-dispersion support; zero at VWAP, unavailable with flat/insufficient dispersion

The composite is clipped to `[-1.0, 1.0]` and converted into an easier-to-read
Bullish/Bearish Score:

```text
Bullish/Bearish Score = ((composite weight + 1.0) / 2.0) × 100
```

A score near 50% is neutral, higher values indicate increasingly bullish
alignment, and lower values indicate increasingly bearish alignment. The
result also includes a market-regime label, normalized factor scores, raw
integral and derivative values, and the ticker's excess returns versus its
benchmarks. This is a directional statistical summary, not a prediction or
investment recommendation.

The desktop application and website use the same calculation inputs. Desktop
analysis reads the active SPY and sector analyzers; the FastAPI analysis route
loads current SPY and mapped sector-ETF returns through the quote cache.
Provider sector names are normalized through the shared sector mapping so
aliases such as `Healthcare`, `Financial Services`, and `Basic Materials`
resolve to the same ETFs in both interfaces.

Statistical analysis is available from:

- The desktop ticker detail window
- **Analyze Selected** in the desktop Watchlist
- **Analyze Selected** in the desktop Market Screener
- The web Analysis page
- Each web Market Screener result
- Each web Watchlist ticker row
- `GET /api/analysis/{ticker}?interval=One`
- The web Model Insight page at `/model-insight`
- `GET /api/model-insight?ticker=MSFT&days=45`

Every analysis view identifies the ticker and sector. The shared desktop
window also presents separate composite and Bullish/Bearish Score cards, the one-minute
timeframe, SPY and sector excess returns, all normalized factor scores, raw
integral and average-derivative values.
Browser dialogs expose the same principal result fields through the shared API
response, keeping web and desktop Bullish/Bearish Scores consistent. The
Model Insight page separately audits recent score distributions, regime
frequencies, factor contributions, and source coverage; it does not compute
forward returns.

For the complete formulas, scaling constants, interpretation matrix, and
implementation details, see [Statistical Analysis.md](./Statistical%20Analysis.md).

### Latest Behavioral Changes

- Charts now use the regular-session open as their reference price instead of
  the previous close.
- Today's regular-session open is taken from the first current-day Nasdaq
  intraday bar, with Nasdaq summary and Yahoo Finance values as fallbacks.
- Before the regular session begins, the open is labeled as the previous
  session's open rather than being presented as today's open.
- Intraday charts include only the active market session: pre-market,
  regular-market, or post-market.
- Every chart starts at a zero baseline, including smoothed curves and fallback
  charts with limited provider data.
- Current-price labels show the current price, absolute price change, and
  percentage change together on desktop and mobile.
- Website and desktop quote panels show only the relevant extended-hours
  session and remain centered and responsive on mobile screens.
- The Analysis quote result aligns exactly with the centered search form at
  desktop and mobile widths instead of shrinking to its text content.
- Desktop Watchlist rows use a dedicated 34-pixel height for improved spacing
  while retaining sortable, symbol-keyed row updates.
- Website chart axes use data-driven intervals that match the desktop chart
  presentation more closely.
- Company logos appear in website charts and desktop chart windows with a
  consistent display size.
- Desktop searches show live and intraday results before year-to-date history
  finishes loading.
- Year-to-date history refreshes in the background and is reused from cache for
  later views.
- Chart interval changes refresh the active chart without a full page reload.
- Pre-market fallback data labels a previous-session open clearly rather than
  presenting it as today's open.
- Desktop extended-hours labels now require an active session and a usable
  price; empty values and `N/A` are omitted from watchlist and chart displays.
- Closing or hiding the desktop sector window does not stop its background
  analyzers; reopening it displays their latest queued values.

### Performance

- Quote metadata, live prices, market status, summaries, and intraday chart data
  now load concurrently.
- Reduced measured desktop launch time from 13–15 seconds to approximately
  5.8 seconds while retaining the requested five-second splash. Startup now
  reads local symbol catalogs, defers Matplotlib/SciPy analysis imports until
  a detail or analysis action is opened, and performs Yahoo authentication on
  the first background request instead of during GUI construction.
- Yahoo credentials and cookies are shared correctly between quote helpers, so
  SPY and sector analyzer creation no longer repeats the authentication
  network request for every ETF.
- Desktop searches return live and intraday data before loading year-to-date
  history.
- Year-to-date history refreshes in the background and remains cached for later
  requests.
- Added chart versioning and cache-aware refreshes to avoid unnecessary redraws.

### Reliability

- Added fallback chart data for periods without provider chart points.
- Preserved previous-session open labeling before the regular market session.
- Added regression coverage for open-price anchoring and zero-based charts.
- Improved error handling for failed chart and background history requests.

### Validation

- Python compilation and JavaScript syntax checks pass.
- Targeted chart and API tests pass where optional runtime dependencies are
  available.

## Version 0.0.7

### Highlights

- **ETF-aware sector-weighted market regime and relative alpha.** Individual
  stocks still compare against one mapped GICS sector ETF, but ETFs now
  compare against a holdings-weighted blend of *every* sector they actually
  hold (sourced from Yahoo Finance's `topHoldings`), since a single-sector
  comparison misrepresents a diversified fund. Missing benchmark data for one
  held sector is excluded and the remaining weights are renormalized rather
  than silently understating the blended return; a `min_coverage` guard
  discards the blend entirely if too little of the ETF's declared weight
  actually resolved.
- **ETF sector-mix display.** Ticker detail windows, the desktop analysis
  window, and the web analysis modal now show a "Sector Mix" breakdown (e.g.
  "Technology 38.7%, Financial Services 12.1%, ..., +6 more (21.1%)") for
  ETFs instead of "Sector: N/A", built from the same holdings data above.
- **Macro Market Environment risk profile.** A new "Market Environment" link
  (web header and desktop main window) opens a modal/window computing a
  0–100 macro credit-risk score from seven FRED-sourced (or fallback)
  inputs — Corporate Debt-to-GDP, Equity Risk Premium (SPY earnings yield vs.
  10-Year Treasury), Fed Funds Pressure, Core PCE Inflation, Credit Spread
  Stress, Yield-Curve Inversion, and Federal Debt-to-GDP — classified into
  Low/Elevated/Critical tranches with the same red/yellow/green badge scheme
  used elsewhere in the app. SPY's trailing P/E is fetched live from Yahoo
  Finance rather than hardcoded; every factor reports whether it used a live
  or fallback value, and the UI visibly flags any estimated figures instead
  of presenting them with the same confidence as live data. The desktop
  window's close control now withdraws (hides) rather than destroys the
  window, so its already-fetched results are stashed and reappear instantly
  on reopen instead of re-fetching from FRED every time.
- **Empirical backtesting for the Market Environment model.** A new
  "Backtest" button reconstructs the macro risk score at every historical
  month back to a user-selected start date (as early as 1990, default 1999)
  through an optional end date, using point-in-time-approximated FRED data
  and real SPY price history, then reports its correlation with SPY's actual
  subsequent 3/6/12-month performance — including a plain-language ✓/✗ check
  for whether riskier months really did precede worse outcomes, color-coded
  correlation values, and a "How to read these results" explainer covering
  what the forward-horizon panels, tranches, and correlation values mean.
  Running this backtest surfaced real, actionable evidence: the model's
  factor weights were rebalanced based on which factors' scores actually
  correlated with subsequent SPY performance in the data (Corporate
  Debt-to-GDP and Equity Risk Premium turned out to be the strongest,
  most consistent predictors and were weighted up; Federal Debt-to-GDP
  correlated in the wrong direction — likely confounded by its near-perfect
  correlation with the passage of time over the sample's long bull market —
  and was weighted down).
- **Fixed a real data bug found while investigating the COVID-19 crash**:
  `BCNSDODNS` (Corporate Debt-to-GDP's debt input) was reporting its
  point-in-time (`output_type=4`) vintage values in a different unit than
  its standard endpoint — roughly 1,000x smaller for the same date, with no
  indication of this in FRED's series metadata — which silently zeroed out
  Corporate Debt-to-GDP (a 25%-weight factor) for every reconstructed month
  from 2010-04-01 onward. The point-in-time fetch now sanity-checks each
  value against the standard series and substitutes it when they differ by
  more than a 10x ratio, a threshold far beyond any plausible real revision.
- Fixed a Tkinter theming bug where every Market Environment progress bar
  rendered green regardless of its actual risk level, because Windows' default
  ttk theme renders `Progressbar` through the OS's native visual-styles engine
  and ignores `ttk.Style` color overrides entirely. Bars are now drawn
  directly on a `tk.Canvas`, which is unaffected by native theming.
- Fixed the per-ticker statistical model's integral (trend-persistence)
  factor, which previously saturated to its extreme value within the first
  hour of almost any session regardless of how large the actual move was,
  because it compared a time-accumulating area against a fixed, time-blind
  threshold. It now divides by the elapsed session span first, making the
  score reflect the *size* of a sustained move rather than just how long the
  session has been running.

### Reliability and cleanup

- Removed dead code accumulated across the project (unused imports, unused
  local variables, an unreachable exception-handling path, and a handful of
  functions with no remaining callers), including a stray byte-order-mark in
  `utils/Nasdaq.py`.
- Fixed several smaller Market Environment issues found along the way: a
  Credit Spread Stress floor set above real-world observed lows (which
  rendered as a fully uncolored bar instead of a small visible one), a
  closure bug where a caught exception's message could be referenced after
  Python auto-unbinds it, and a crash when a historical backtest month had no
  forward-looking price window at all.

### Validation

- Full automated test suite: 90 tests passing (17 pre-existing skips due to
  an unrelated local FastAPI environment gap), up from the prior release's
  baseline, including new coverage for the ETF sector-weighting math, the
  Market Environment risk profile and its live-data fallbacks, and the
  backtest's date validation, point-in-time data reconstruction, and the
  units-scale-mismatch repair.
- Verified the Market Environment window's hide/reopen behavior and the
  progress-bar coloring fix with headless Tkinter smoke tests driving a real
  `mainloop`.

## Version 0.0.7

### Highlights

- **Ticker-vs-market comparison for the Backtest.** The Backtest modal's
  "Compare:" toggle now switches between "Market only (SPY)" and "Ticker
  vs. market" mode. In ticker mode, the underlying market-environment
  backtest runs unchanged — the risk-score reconstruction is purely
  macro/FRED-based and never depends on which ticker is chosen — but the
  modal also fetches the chosen ticker's own historical price history and
  adds, per risk tranche, that ticker's mean forward return and drawdown,
  its excess return over SPY, and the sample size behind those figures,
  plus overall correlation values for the ticker itself. The ticker is
  validated against the existing symbol catalog, and results are cached on
  the full `(start_date, end_date, ticker)` combination so repeat requests
  are served instantly. `GET /api/backtest-risk-profile` accepts an
  optional `?ticker=` query parameter, and
  `Market_Analysis/backtest_risk_profile.py`'s SPY-only price-history
  fetcher was generalized to work for any valid ticker.
- **Nasdaq market-status resilience.** `NasdaqDataFetcher.check_market_status()`
  now retries transient 403/429 responses from Nasdaq's unofficial
  `market-info` endpoint with escalating backoff — mirroring the retry
  pattern already used elsewhere for year-to-date returns — and, if Nasdaq
  is still unavailable once retries are exhausted, falls back to Yahoo
  Finance's `marketState` (mapped to Nasdaq's own status vocabulary), then
  to the last known cached status, before finally surfacing an error. A
  single blocked or rate-limited Nasdaq request no longer breaks market-status
  detection for the whole app.
- Backtest modal UI polish: the "Compare:" mode radio buttons and their
  labels are now reliably aligned across browsers, the ticker entry field
  lives inline with the mode toggle instead of in its own separate row, and
  the mobile layout (radio dot size, label/column alignment, and ticker
  input sizing) was reworked to stay correct across the entire mobile width
  range rather than only at one specific breakpoint.

### Validation

- Full automated test suite: 107 tests passing (17 pre-existing skips due to
  an unrelated local FastAPI environment gap), including new coverage for
  the ticker comparison math and generalized price-history fetch
  (`Market_Analysis/Unit Test/test_backtest_risk_profile.py`) and the
  Nasdaq retry/Yahoo-fallback chain (`utils/Unit Test/test_nasdaq.py`).

## Code documentation map

The implementation is divided by responsibility:

- `common/request_quotes.py` coordinates provider requests and normalizes
  market-session state.
- `common/sectors.py` maps sectors/Yahoo holdings keys to benchmark ETFs,
  blends an ETF's holdings-weighted sector return, and formats its sector-mix
  breakdown for display.
- `utils/Yahoo_Finance_helper.py` handles Yahoo live quotes, paginated
  screener records, ETF holdings/trailing-P/E lookups, and provider-field
  fallbacks.
- `Market_Analysis/market_analyzer.py` computes the macro Market Environment
  risk profile (`generate_spy_risk_profile`) from FRED/Yahoo inputs.
- `Market_Analysis/backtest_risk_profile.py` reconstructs that risk profile
  across historical months and correlates it with SPY's actual subsequent
  performance, optionally comparing a chosen ticker's own forward returns
  against SPY for each risk tranche.
- `Fast_API/stock_api.py` exposes HTML routes and JSON APIs for quotes, charts,
  screener results, statistical analysis, descriptive model insight, the Market
  Environment profile, and the historical backtest (with optional per-ticker
  comparison).
- `Interface/pensive_trader_display.py` owns the desktop search, watchlist,
  screener/sector/market-environment toggles, analyzer queue display, worker
  queue, logo loading, and sortable result Treeview.
- `Interface/watchlist_interface.py`, `Interface/root_top_level.py`,
  `Interface/analysis_window.py`, `Interface/market_environment_window.py`,
  and `Math/price_plot.py` render desktop watchlist, detail,
  statistical-analysis, macro-risk, and chart surfaces.
- `JavaScript_Interface/` contains browser rendering and interaction logic;
  `common.js` renders shared statistical-analysis, Market Environment, and
  Backtest dialogs, `calibration.js` renders the descriptive Model Insight page,
  while `screener.js` handles normalized screener rows, row-level analysis
  actions, and client-side sorting.
- `CSS_Interface/` contains shared, page-specific, and responsive styles.

When changing a shared behavior, update the owning layer first and verify both
frontends. Extended-hours output must be gated by both the active market state
and a non-empty, non-`N/A` value.

## Notes

- The desktop application uses Tkinter and opens popup windows for detailed
  ticker information.
- Package versions and environment differences may require additional
  dependencies for local builds.

## License

This release is not for commercial use unless explicitly authorized by the
owner.
