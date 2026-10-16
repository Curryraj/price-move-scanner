# price-move-scanner — Spec

## Goal
Alert me when a stock goes up or down by 4% or more since its last check.
One check per run (no loop yet; a loop can come later).

## Input
- `tickers.txt`: the stock symbols I choose, one per line (I write this file)
- Threshold: default 4%, changeable when running (`--threshold`)
- New price: from the Finnhub API
- Last price: the most recent row for that ticker in `prices.csv`

## Output
- Terminal alert, only for tickers that moved enough:
  `AAPL +5.0%  OLD 100.00 → NEW 105.00`
- `prices.csv`: every check is saved, one row each, columns `ticker, price, time`

## Rules
- Percentage change = `((new − old) / old) × 100`
- Alert if the change is 4% or more, either up or down
- First run (no previous price for a ticker): save the price, no alert
- API error or bad ticker: skip that ticker and carry on with the others
- Old price can never be zero; if it is, skip the comparison for that ticker
- Last price = the previous price read for that ticker

## Done when
- old 100, new 105 → alert (+5%)
- old 100, new 96 → alert (−4%)
- old 100, new 101 → no alert (+1%)
- old 100, new 104 → alert (exactly 4% counts)
- a bad ticker alongside good ones → the good ones still work
- first run → no crash, price saved, no alert
