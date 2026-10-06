# Bollinger Band Mean Reversion

**Notebook:** `bollinger_band_mean_reversion_backtest.ipynb`

## Idea
Prices that stretch far below their recent average tend to snap back. Bollinger Bands (a moving average ± k standard deviations) define "too far," and the strategy buys the dip and exits on the reversion.

## Methodology
1. **Data** – Daily closes from Yahoo Finance (`yfinance`).
2. **Bands** – Middle = N-day SMA; Upper/Lower = SMA ± k·σ (default N = 20, k = 2).
3. **Entry** – Go long when the close falls below the lower band.
4. **Exit** – Sell when price crosses back above the middle band (SMA), or on a stop-loss (final version).
5. **Sizing** – Fixed fraction of current equity per trade (1–15% depending on version).
6. **Metrics** – Total return, annualized Sharpe, max drawdown, win rate, and alpha vs. the S&P 500 (`^GSPC`).

## Iterations in the notebook
| Cell | Universe | Twist |
|---|---|---|
| 1 | SPY + ASML | Buy ASML only when **both** SPY and ASML break their lower bands |
| 2 | UNG, SVXY, IEF, XLE, NVDA | Per-asset band parameters across asset classes |
| 3 | UNG, AMD (hourly) | Volume confirmation: entry requires above-average volume |
| 4 | XLE + AMD, 2020–2025 | Portfolio version with 15% sizing, 50% stop, Sharpe/alpha/drawdown |

## Known limitations
- No transaction costs or slippage.
- Cell 1's Sharpe ratio is inflated by how daily returns are computed; treat it as illustrative.
- Cell 2 fails on newer `yfinance` (multi-index columns): `data.empty` / `close` are DataFrames — use `close = data['Close'].squeeze()`.
- Cell 3's hourly data is only available for the last ~730 days; move the date range up to rerun.

## Run
```bash
pip install yfinance pandas numpy matplotlib scipy
```
