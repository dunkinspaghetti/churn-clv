# DATA.md

Source: UCI Machine Learning Repository, Online Retail II, https://archive.ics.uci.edu/dataset/502/online+retail+ii
Licence: CC BY 4.0
Downloaded: 2026-11-13

The raw file is not committed. Download it into data/ as online_retail_II.zip.

## Row counts

- Raw rows (both sheets): 1,067,371
- No Customer ID, dropped: 243,007 rows (22.8% of rows, 13.7% of gross revenue)
- cancellation invoice (Invoice starts with C): 18,744 rows removed
- Quantity <= 0: 0 rows removed
- Price <= 0: 71 rows removed
- non-product StockCode: 2,915 rows removed
- Clean rows: 802,634
- Orders (customer x invoice): 36,594
- Customers: 5,852

## Modeling choices

- T_end = 2011-12-09, churn window N = 148 days (90th percentile gap), sensitivity at the 75th and 95th
- New-customer cutoff: first order on or after 2010-06-01, removed 2,684 customers
- First 30 days must be fully observed before T_end, removed 152 customers
- Customers modelled: 3,016
- Gaps are measured between distinct order days

## Act 2: CLV (BG/NBD + Gamma-Gamma)

- Same source, licence and cleaning as Act 1 (rules above); clean rows: 802,634
- Unit: one customer, one order day; order days 32,878, customers 5,852
- Calibration ends 2011-06-12, holdout 180 days to 2011-12-09
- Horizon 12 months, discount 10% a year, revenue in GBP (no cost data, so not profit)
- Gamma-Gamma independence check: correlation of frequency and average order value among repeaters = 0.084
- Holdout revenue: predicted 3,779,301 vs actual 4,510,530
- Total 12-month CLV 8,888,115; top 10 percent of customers hold 60.3%

## Act 3: segments and retention budget

- Same cleaning and same BG/NBD as Act 2; total 12-month CLV 8,888,115
- Churn risk = probability of no order in the next 365 days under BG/NBD (exact formula). Hidden-180-day check: predicted 48.3% silent, actual 48.1%
- Value at stake = saveable risk x value if kept (average order value x order rate x 365 days, undiscounted); total 826,741
- Rank correlation of risk with CLV: -0.86
- Assumptions (invented, not from the data): budget 50,000 GBP, 25 GBP per contact, save rate 15%
- Risk bands: low below 25%, mid 25% to 60%, high above 60%; value tiers are the Act 2 CLV quartiles
