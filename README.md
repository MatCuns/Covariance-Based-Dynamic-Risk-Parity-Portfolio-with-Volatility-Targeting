# EUR Risk Parity + Volatility Targeting (2001–2026)

In this pyhton project I build a fully EUR-denominated portfolio that combines:

• a diversified **risky multi-asset European basket** constructed with **Risk Parity**, and  
• a defensive sleeve based on the **Bloomberg Euro Aggregate Government Bond Index**,  

and I apply **rolling 60-day portfolio-level volatility targeting** without leverage.

I design the portfolio using **1990–2000 data** and evaluate it **out-of-sample from 2001 to 2026**, comparing it to the **STOXX Europe 600 Total Return Index**.

---

## Data

I use **daily total return price CSVs (dividends included)** in EUR.

I require:

• 20 European risky assets from different sectors (including commodities exposure)  
• Bloomberg Euro Aggregate Government Bond Index  
• STOXX Europe 600 Total Return Index  

I align all series on common trading days.

---

## Portfolio Construction

### Step 1 — Asset universe

I select **20 European risky assets** representing different sectors.  
All assets are EUR-denominated total return series.

I also include one defensive asset:

• Bloomberg Euro Aggregate Government Bond Index.

---

### Step 2 — Initial portfolio design (1990–2000)

Using daily returns from **1990–2000**, I:

1. Compute the covariance matrix of the 20 risky assets.
2. Solve for **Risk Parity weights** so that each asset contributes equally to portfolio variance.

This gives weights $$( w_i \)$$ for the risky basket.

At the start of 2000 I set:

• Bond weight = **10%**  
• Risky basket weight = **90%**

---

### Step 3 — Out-of-sample period (2001–2026)

I impose a **no-leverage constraint**.  
The total risky exposure never exceeds 100%.

This project focuses on risk control.

---

### Step 4 — Volatility targeting

I estimate portfolio volatility using a **rolling 60-day window** of past returns.

Target volatility = **12%**.

At each rebalance date I:

1. Compute realized portfolio volatility over the previous 60 trading days.
2. Compare it with the 12% target.
3. Reallocate between the risky basket and the bond sleeve.

If volatility is above target → I increase bond weight.  
If volatility is below target → I increase risky weight.

I keep the relative weights inside the risky basket unchanged during this step.

I never allow leverage.

---

### Step 5 — Risk Parity refresh every 5 years

Every 5 years I recompute the risky basket:

1. I use the previous 5 years of daily data.
2. I recompute the covariance matrix.
3. I solve for new Risk Parity weights.
4. I update the risky basket allocation.

This allows the portfolio structure to adapt to changing correlations.

Example schedule:
• 2005 uses 2000–2004  
• 2010 uses 2005–2009  
• 2015 uses 2010–2014  
• 2020 uses 2015–2019  
• 2025 uses 2020–2024  

---

### Step 6 — Trading costs

I include transaction costs based on **Interactive Brokers commission levels**, applied to traded notional at each rebalance.

This prevents unrealistic performance caused by excessive trading.

---

## Benchmark

I compare performance to:

• STOXX Europe 600 Total Return Index (EUR)

from 2001 to 2026.

---

## Performance Metrics

I compute:

• Compound Annual Growth Rate (CAGR)  
• Annualized volatility  
• Sharpe ratio  
• Sortino ratio  
• Maximum drawdown  
• Calmar ratio  
• Total turnover  
• Total transaction costs  

And I compare it with the benchmark.

---

## Methodology Safeguards

I avoid look-ahead bias by using only past data.  
I separate in-sample (1990–2000) from out-of-sample (2001–2026).  
I keep all series in EUR to avoid FX effects.

---

## Disclaimer

This repository is for academic research purposes only.  
Backtested performance does not guarantee future results.
