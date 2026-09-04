# For-Release
This is a repo to push release of apps being developed while keeping secret sauce private, for now

# Stock Screener

A Tkinter-based stock screener that lets you search stock tickers, track price changes, and open detail windows with additional market information and chart views.

## Features

- Search up to 5 ticker symbols at a time
- View live price, change, and percent change information
- Open individual ticker detail windows
- View chart options for different time ranges
- Launch a desktop GUI with a splash screen

## Latest release changes

- Added a FastAPI web dashboard alongside the Tkinter desktop application.
- Added responsive quote and chart pages for desktop and mobile layouts.
- Added asynchronous JSON refreshes for quote data and chart interval changes.
- Added shared navigation and responsive styling across the HTML pages.
- Added bundled splash-screen and background image resources for PyInstaller builds.
- Updated the PyInstaller spec and build helper to include the `Images/` directory.
- Added request-local ticker handling so different ticker submissions do not reuse stale chart data.

## Notes

- The app uses Tkinter for the interface and creates popup windows for detailed ticker information.
- Some package versions and environment differences may require installing additional dependencies depending on your Python setup.
- The latest

## Live Website
You can view the live project here: [Pensive Trader](https://stock-screener.fastapicloud.dev/)

## License

This release is not to be used for commercial purposes unless explicitly stated by the owner.
