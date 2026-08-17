# SPY Options Market-Making Analysis

An exploratory analysis of the SPY options chain from a **market-maker's perspective** — where spread income is earned, how inventory is priced (the volatility surface), whether the surface is arbitrage-consistent, and what risk a market maker accumulates.

**Data:** 5 daily end-of-day SPY option-chain snapshots (2022-08-01 → 08-05), 22,962 rows, 36 expiries (0DTE to ~872d), strikes 85–720. Source: OptionsDX wide-format.

**Stack:** Python · pandas · NumPy · matplotlib

---

## Analysis Structure

### 1. Data Cleaning & Market-Making Indicators
Cleaned malformed column names and coerced pseudo-numeric quote columns; engineered mid price, absolute/relative spread, and moneyness. Relative spread `(ask−bid)/mid` is used throughout for cross-strike comparability.

### 2. Spread Analysis — Liquidity Structure
Median relative spread mapped across moneyness × DTE reveals a **liquidity smile** (tightest ATM, widening into both wings) and a **U-shaped term structure** (widest at 0–7d and 181–365d, tightest in the 31–180d sweet spot). A `mid ≥ $0.10` floor is applied to avoid tick-size artifacts that inflate cheap-option spreads to 100–200%.

### 3. Volatility Surface
- **Smile / skew:** strong equity **downside skew** (put IV richest), steepest short-dated in moneyness space.
- **ATM term structure:** upward-sloping (contango) — the calm-market shape.
- **25Δ risk reversal:** positive across all tenors, largest long-dated in delta space (long-dated puts are the institutional tail hedge). Reconciles why skew looks steepest short-dated in *moneyness* but largest long-dated in *delta*.

### 4. Put-Call Parity — No-Arbitrage Check
Per-expiry linear regression of `C−P` vs strike. RMS residuals of ~2–20¢ confirm the quoted surface is arbitrage-consistent. Extracts a robust **forward curve**; notes that a single-factor fit cannot cleanly separate the risk-free rate from the dividend yield.

### 5. Greek Risk Maps
Gamma / vega / theta heatmaps across the surface. Key finding: **gamma concentrates at short-dated ATM, vega at long-dated ATM** — the diagonal separation that makes hedging and inventory limits tenor-specific.

---

## Files
- `option.ipynb` — full analysis notebook
- `spy_options_data.csv` — SPY options chain data

## Key Concepts
Market-maker microstructure · implied volatility surface · volatility skew · put-call parity · option Greeks · inventory & adverse-selection risk
