# USD Risk Parity + Volatility Targeting (2000–2026)

In this python project I build a USD portfolio that combines:

• A diversified **risky multi asset basket** constructed with **Risk Parity**, and updated every trading year

• A defensive sleeve based on the **3 Month US Treasury Bill**,  

and I apply a **rolling 60 day volatility targeting on the risky assets portfolio** without leverage.

I design the portfolio using **1990–2000 data** and evaluate it **out of sample from 2000 to 2026**, comparing it to the S&P 500 as benchmark.

The project supposes an investment of **$10,000** on the 1st of January 2001 and doesn't include further future investments or withdrawals.
---

## Data

I use **daily total return price CSVs (dividends included)** in USD.
The assets are:

As bonds -> 3 Month US Treasury Bill  
 
As commodities: Gold, Oil, Copper. These represent three different parts of the commodity market. Gold as safe haven and inflation dynamics, oil captures the energy market and global demand shocks, and copper represents industrial activity and the global economic cycle.

For stocks: I pick the HIGHEST USD MarketCap i could find per sector around 1990 (to avoid look ahead bias as much as possibile), the sectors and industry groups are chosen following GICS (https://www.msci.com/indexes/index-resources/gics) :
Information Technology -> IBM, 
Financials -> Citigroup, 
Health Care -> Merck & Co,
Industrials -> General Electric Co,
Consumer Discretionary -> McDonald's, 
Consumer Staples -> Procter & Gamble,
Energy -> Exxon, 
Materials -> DuPont De Nemours Inc, 
Utilities -> Southern Company, 
Communication Services -> AT&T, 
Retail-> Walmart,
Banks -> Bank of America, 
Insurance -> AIG, 
Capital Goods -> Boeing,
Transportation -> FedEx,
Automobiles & Components -> Ford,
Food, Beverage & Tobacco -> Coca Cola.

I align all series on common trading days and forward fill with a maximum of 1 day NaN.

---

## Portfolio Construction

### Step 1 — Asset universe

I download the csv of the **20 assets** representing different sectors from Stooq website https://stooq.com/.  
Oil spot price csv is downloaded from FRED website https://fred.stlouisfed.org/series/DCOILWTICO, excluding 20 april 2020 (negative price)
All assets are USD.

I also include one defensive asset:

• 3 Month US Treasury Bill from FRED website, on which i will compute the daily yields  and use it as risk free rate https://fred.stlouisfed.org/series/DTB3

---

### Step 2 — Initial portfolio design (1990–2000)

Using daily returns from **1990–1999**, I:

1. Compute the covariance matrix of the 20 risky assets.
2. Solve for **Risk Parity weights** so that each asset contributes equally to portfolio variance.

This gives weights $$( w_i \)$$ for the risky basket.

At the start of 2000 I set:

• Bonds weight = **10%**  
• Risky basket weight = **90%**

---

### Step 3 — Out of sample period (2000–2026)

I impose a **no leverage constraint**.  
The total risky exposure never exceeds 100%.

This project focuses on risk control.

---

### Step 4 — Volatility targeting

I estimate portfolio volatility using a **rolling 60 day window** of past returns.

Target volatility = **12%** checked daily.

At each rebalance I:

1. Compute realized portfolio volatility over the previous 60 trading days.
2. Compare it with the 12% target.
3. Reallocate between the risky basket and the bond sleeve.
4. Deduct transaction costs

If volatility is above target → I increase bond weight and reduce risky exposure.  
If volatility is below target → I increase risky weight and reduce the bond sleeve.

I keep the relative weights inside the risky basket unchanged during this step.

I never allow leverage.

---

### Step 5 — Risk Parity refresh every year

Every trading year I recompute the risky basket:

1. I use the previous 1 year of daily data.
2. I recompute the covariance matrix.
3. I solve for new Risk Parity weights.
4. I update the risky basket allocation.

This allows the portfolio structure to adapt to changing correlations.

Example schedule:  
• 2005 uses 2004  
• 2006 uses 2005 
• 2025 uses 2024  
and so on

---

### Step 6 — Trading costs and Slippage

I include transaction costs based on **Interactive Brokers commission levels**, applied at each rebalance.
https://www.interactivebrokers.com/en/pricing/commissions-stocks.php 
giving that we are dealing with USD let's imagine that we are US based and take those commissions.

Slippage
Given that we work with higly liquid stocks I will assume a 2bp slippage (-0.02% per trade)

---

## Benchmark

I compare performance to the S&P 500 Index from 2001 to 2026.

---

## Performance Metrics

I compute:

• Compound Annual Growth Rate (CAGR)  
• Annualized volatility  
• Sharpe ratio    
• Maximum drawdown  
• Total transaction costs  

And I compare it with the benchmark.
My goal is to have a lower risk and higher sharpe ratio than the market/benchmark , not to beat it in terms of returns.

---

## Disclaimer

This repository is for academic research purposes only.  
Backtested performance does not guarantee future results.
