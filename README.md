# USD Risk Parity + Volatility Targeting (2001–2026)

In this python project I build a USD portfolio that combines:

• a diversified **risky multi-asset basket** constructed with **Risk Parity**, and  
• a defensive sleeve based on the **Bloomberg U.S. Aggregate Government Bond Index**,  

and I apply a **rolling 60-day portfolio-level volatility targeting** without leverage.

I design the portfolio using **1990–2000 data** and evaluate it **out of sample from 2001 to 2026**, comparing it to a broad global equity benchmark in USD.

The project supposes an investment of **$10,000** on the 1st of January 2001 and doesn't include further future investments or withdrawals.
---

## Data

I use **daily total return price CSVs (dividends included)** in USD.
The assets are:

As bonds -> Bloomberg U.S. Aggregate Government Bond Index  
 
As Commodities: Gold, Silver, Copper

For stocks: I pick the HIGHEST USD MarketCap i could find per sector around 1990 (to avoid look ahead bias as much as possibile), the sectors and industry groups are chosen following GICS (https://www.msci.com/indexes/index-resources/gics) :
Information Technology -> IBM, 
Financials -> Citigroup, 
Health Care -> Merck & Co,
Industrials -> General Electric Aerospace,
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

I align all series on common trading days.

---

## Portfolio Construction

### Step 1 — Asset universe

I download the csv of the **20 assets** representing different sectors.  
All assets are USD-denominated total return series.

I also include one defensive asset:

• Bloomberg U.S. Aggregate Government Bond Index.

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

### Step 3 — Out of sample period (2001–2026)

I impose a **no leverage constraint**.  
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

I include transaction costs based on **Interactive Brokers commission levels**, applied at each rebalance.

This prevents unrealistic performance caused by excessive trading.

---

## Benchmark

I compare performance to the S&P 500 Index from 2001 to 2026.

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

## Methodology

I avoid look-ahead bias by using only past data.  
I separate in-sample (1990–2000) from out-of-sample (2001–2026).  
I keep all series in USD for consistency.
My goal is to have a lower risk and higher sharpe ratio than the amrket, not to beat it.
---

## Disclaimer

This repository is for academic research purposes only.  
Backtested performance does not guarantee future results.
