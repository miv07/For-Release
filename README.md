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

## Version 0.0.3

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

### Performance

- Quote metadata, live prices, market status, summaries, and intraday chart data
  now load concurrently.
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
