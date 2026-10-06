# Implied vs. Realized Volatility — Discount Retail

**Notebook:** `implied_vs_realized_volatility.ipynb`

## Idea
Compare the volatility the options market is *pricing in* (implied volatility, IV) with the volatility the stock *actually delivered* (realized volatility, RV). A persistent gap suggests options are rich (IV > RV → favor selling premium) or cheap (IV < RV → favor buying).

## Methodology
1. **Universe** – Discount/grocery retailers: Dollar General (DG), Walmart (WMT), Kroger (KR).
2. **Price data** – Daily closes for 2024 via `yfinance`; 10- and 50-day moving averages for trend context.
3. **Realized volatility** – 20-day rolling std. dev. of daily returns, annualized by √252.
4. **Implied volatility** – Pulled from the current option chain (`yf.Ticker(...).option_chain`) across expirations; averaged across calls/puts, and grouped by expiration to view an IV term structure.
5. **Comparison** – Plot rolling RV against the average IV level and inspect the spread.

## Known limitations
- `yfinance` only returns the **current** option chain, so IV is a snapshot, not a historical series. A true IV-vs-RV time series needs historical options data (e.g., OptionMetrics/WRDS, CBOE).
- Averaging IV across all strikes mixes deep ITM/OTM contracts; ATM IV is a cleaner measure.

## Run
```bash
pip install yfinance pandas matplotlib
```
