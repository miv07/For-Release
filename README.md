# For-Release

This repository contains app releases while the project's private implementation
details remain separate.

# Pensive Trader

A Tkinter-based stock screener for searching ticker symbols, tracking price
changes, opening detail windows, and viewing market charts. It also includes a
FastAPI web dashboard for responsive quote and chart views.

## Features

- Search up to 5 ticker symbols at a time.
- View live price, change, and percentage change information.
- Open individual ticker detail windows.
- View chart options for different time ranges.
- Launch a desktop GUI with a splash screen.
- Use responsive quote and chart pages on desktop and mobile.
- View extended-hours quote data across the GUI, desktop app, and mobile website.
  - Pre-market price and change information.
  - Post-market price and change information.
- Display only the active extended session in each interface.
  - Pre-market views show only pre-market values.
  - Post-market views show only post-market values.
  - Regular and closed-market views hide extended-hours fields.
- Audit the per-ticker Bullish/Bearish Score with a website-facing
  Model Insight Report that summarizes score distribution, regime
  frequency, factor influence, and data coverage without claiming predictive
  accuracy.

## Current Development Notes

- Added `/model-insight`, a dedicated web page for the Model Insight
  Report. The page accepts a ticker and lookback window, explains that the
  report audits model behavior rather than forecasting returns, and renders the
  API response as readable summary, interval, regime, factor, and coverage
  cards.
- Added `GET /api/model-insight`, backed by
  `Math/descriptive_calibration.py`, to sample recent historical intraday bars
  and summarize Bullish/Bearish Score distributions, composite-weight
  distributions, regime frequencies, mean factor scores, mean factor
  contributions, and source coverage.
- Added Yahoo historical intraday bar support for calibration and reused the
  historical cumulative-volume profile concept so relative-volume availability
  is visible instead of silently neutral.
- Renamed user-facing directional wording from conviction/directional position
  to **Bullish/Bearish Score**. Compatibility aliases remain in the API for
  older callers.
- Improved the per-ticker analysis model with interval-matched Relative Alpha
  and historical intraday relative-volume confirmation. Missing benchmark or
  volume data is reported through source/coverage fields instead of being
  treated as neutral evidence.

## Version 0.0.7

### Highlights

- Added a FastAPI web dashboard alongside the Tkinter desktop application.
- Added responsive quote and chart pages for desktop and mobile layouts.
- Added asynchronous JSON refreshes for quote data and chart interval changes.
- Added shared navigation and responsive styling across the HTML pages.
- Added desktop chart logos and hover-based price inspection.
- Standardized chart baselines so charts start at zero and use the session open
  as the price anchor.
- Added absolute price change alongside current price and percentage change.
- Matched website chart axis intervals to the desktop chart presentation.
- Restored Financial Modeling Prep logo URLs across web and desktop charts.
- Added bundled splash-screen and background image resources for PyInstaller
  builds.
- Updated the PyInstaller spec and build helper to include image resources.
- Added request-local ticker handling to prevent stale chart data reuse.
- Added a persistent watchlist (backed by `localStorage`) shared between the
  standalone Watchlist page and the screener's "View Watchlist" modal, with
  ticker logos and price-direction coloring on every row.
- Restricted website charts to regular trading hours (9:30 AM - 4:00 PM ET),
  matching the "current session only" behavior across every chart surface.
- Consolidated duplicated front-end logic (clear-input buttons, modal
  open/close wiring, chart logos, price-direction coloring) into a shared
  `common.js` helper module used by every page script.
- Split the monolithic `pensive_trader.css` into focused stylesheets
  (`modals.css`, `forms.css`, `pages.css`, `mobile.css`) imported from a
  slim base file, so each concern can be maintained independently.
- Standardized action-button sizing on mobile via a shared `.action-button`
  class so every primary button (Get Quote, Add, Search, Open
  Chart, View Watchlist, Clear All, etc.) has identical dimensions.
- Removed the Wikipedia/Wikidata "Company Search" feature (route, page,
  scripts, styles, and the `wikipedia-api` dependency) as unnecessary bloat;
  the quote panel no longer links out to a research page.
- Added a Yahoo Finance Market Screener page and persistent desktop screener
  window with matching Filter, Percent change, and Screen Stocks controls.
- Added screener result logos, normalized price/change/volume/market-cap data,
  and clickable sorting for both web and desktop result tables.
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

Version 0.0.5 introduces a bounded statistical weighting model that combines
four views of a ticker's current behavior:

| Factor | Input | Purpose
| --- | --- | --- |
| Relative alpha | Interval-matched return differences versus SPY and the mapped sector ETF | Measures market and peer outperformance on the selected timeframe
| Integral persistence | Average normalized area represented by `current_integral` | Measures whether recent direction is sustained without time-of-day saturation
| Derivative velocity | Spline slope represented by `avg_derivative` | Measures immediate directional momentum
| Relative volume | Current cumulative volume versus expected historical minute-of-day cumulative volume | Confirms whether participation supports the existing price direction

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

## Live Website

Visit the live project: [Pensive Trader](https://stock-screener.fastapicloud.dev/)

## License

This release is not for commercial use unless explicitly authorized by the
owner.
