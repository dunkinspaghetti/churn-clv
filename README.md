# churn-clv

Customer churn, lifetime value and a retention budget on the UCI Online Retail II data (a UK online gift wholesaler, 2009 to 2011). Three notebooks, each ending in one decision.

Raw data is not committed, see DATA.md for the source, licence and every cleaning rule.

## What each act shows

- Act 1, survival (`notebooks/act1_survival.ipynb`): Kaplan-Meier and Cox hazard ratios on 3,016 newer customers. Churn is an inactivity window of 148 days (the 90th percentile gap between orders), a modelling choice tested at the 75th and 95th percentiles.
- Act 2, lifetime value (`notebooks/act2_clv.ipynb`): BG/NBD for orders. Gamma-Gamma for order value failed its own check (shape 0.88, below 1, so no finite average) and was dropped for each customer's own average. On a hidden 180 days, orders were forecast within 1.7 percent but revenue came in 16 percent under. Total 12-month value is GBP 8.89M for 5,852 customers, revenue not profit.
- Act 3, retention budget (`notebooks/act3_segments.ipynb`): churn risk and value come from one model and move opposite ways (rank correlation -0.86), so the textbook cell of high risk and high value holds 3 customers. With a budget of GBP 50,000 and 2,000 contacts, ranking by risk loses GBP 12,107 while ranking by value at stake (risk that can be saved times value if kept) nets GBP 40,860.

## Honest limits

- The budget, the GBP 25 cost per contact and the 15 percent save rate are invented parameters in the first code cell of Act 3. The ranking of segments holds up on the hidden window; the pound figures do not.
- Revenue only, with returns removed rather than netted off. One retailer, one test window.
- The data says who may leave, not why.

## Run it

    pip install -r requirements.txt
    # download online_retail_II.zip into data/ (see DATA.md), then run the notebooks in order

MIT licence.
