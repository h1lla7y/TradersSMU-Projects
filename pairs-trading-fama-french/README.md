# Factor-Based Pairs Trading (Fama-French 3-Factor)

**Notebook:** `pairs_trading_ff3.ipynb`

## Idea
Two stocks with nearly identical risk exposures should move together. When their price spread diverges, bet on convergence: short the rich leg, buy the cheap leg — a market-neutral trade.

## Methodology
1. **Universe** – 18 US retailers (AEO, TJX, DDS, GAP, URBN, WSM, WMT, TGT, BBY, ROST, M, NKE, ULTA, DKS, RL, BBWI, BURL, DLTR), monthly returns since 2015.
2. **Factor regression** – Regress each stock's excess return on the Fama-French 3 factors (Mkt-RF, SMB, HML) with OLS to estimate factor betas.
3. **Pair selection** – Euclidean distance between stocks' beta vectors (`scipy.pdist`); smallest distances = most similar risk profiles. Shown as a heatmap.
4. **Selected pairs** – TGT/NKE, WSM/ULTA, BBWI/BBY, AEO/BURL, ROST/TJX.
5. **Hedge ratio** – OLS of A's price on B's price over a 1-year lookback; spread = A − β·B.
6. **Signals** – 60-day rolling z-score of the spread. Enter at |z| > 1.5 (short spread if high, long if low); exit when |z| < 0.5.
7. **Backtest & execution** – 3-year backtest with per-pair and portfolio Sharpe ratios; paper trades placed through the Alpaca API with a 7-day holding period.

## Setup
Alpaca keys are read from environment variables — never commit them:
```bash
export ALPACA_API_KEY=...
export ALPACA_SECRET_KEY=...
pip install yfinance pandas numpy statsmodels scipy seaborn matplotlib alpaca-py
```

## Known limitations
- Similar factor betas ≠ cointegration; adding an Engle-Granger/ADF test would strengthen pair selection.
- Backtest PnL is in spread units and ignores borrow costs, commissions, and slippage.
