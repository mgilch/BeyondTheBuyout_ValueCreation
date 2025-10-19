Project Overview

This project is split into two parts:
1. KS-PME analysis (Kaplan & Schoar, 2005)
2. Value creation analysis (Puche, Braun, & Achleitner, 2015)

----------------------------------------
1) KS-PME analysis

Requirements
Place the following files in the specified folders:
- InputData/CoreData.xlsx
- InputData/currencies.csv
- InputData/indices.csv
- GrossToNet/Data/deal_to_fund.csv

Run in order:

1. GrossToNet/01GNDataPipeline.ipynb
Handles data loading and cleaning for the PME analysis.
- Ensure deal-to-fund mapping,
- Drop unreasonable dates,
- Ensure fund and deal coverage lies within benchmark and currency coverage,
- Ensure at most one Fair Value/NAV per deal/fund as the terminal valuation.
No user changes required.

2. GrossToNet/02GrossToNetAnalysis.ipynb
Handles:
- Currency harmonization,
- Composite benchmark construction,
- PME engine,
- PME analysis at pooled level and fund (gross and net) level.
No user changes required.

----------------------------------------
2) Value creation analysis

Requirements
Place the following files in InputData:
- InputData/CoreData.xlsx
- InputData/monthly_interest_rates_curr.csv

Run in order:

1. ValueCreation/01DataPipeline.ipynb
Handles data loading and working.csv construction.
- Loads deal time-series data,
- Enriches with deal data,
- Enriches with fund data.
No user changes required.

2. ValueCreation/02DataTransformation.ipynb
Handles data cleaning and value-creation input ratio computation.
- Adds holding_status,
- Filters unreasonable dates,
- Ensures a clean EV-EqV bridge and >0 EBITDA,
- Computes missing bridge items if 2/3 are available,
- Matches date-to-financials for exited and unexited deals and checks currency integrity,
- Calculates value-creation ratios (e.g., EBITDA multiple) for entry and exit and drops unreasonable metrics.

Toggles (use Cmd+F to find; items in parentheses are best left as is):
- (ENFORCE_MIN_ROWS_PER_DEAL option to exclude all deal_ids with fewer than 2 time-series dates.)
- HOLDING_FILTER_MODE option to run the analysis on all deals or exited_only deals.
- (REQUIRE_POSITIVE_EBITDA option to also include deals with negative EBITDA.)
- (ENFORCE_POSITIVE_EV_AND_EQ option to allow negative bridge items.)
- (Specify which deals will be dropped option to customize cutoffs for ratios.)

3. ValueCreation/03ValueCreationCalculationTotalSample.ipynb
Handles value-bridge item computation.
- Collapses every deal to only entry and exit rows (ensures 2 observations per deal),
- Calculates TM (levered), Leverage Effect, and TM (unlevered),
- Calculates all other effects,
- Computes all items in TM-points,
- Computes all items as share of times_money (levered).

Toggle (use Cmd+F to find; best left as is):
- (APPLY_TM_SHORT_HOLD_FILTER to allow filtering of deals with short holding periods and near-0 times_money.)

4. ValueCreation/TotalSampleValueCreation and ValueCreation/ValueCreationBreakdown
These folders contain the visualization scripts for the value-creation bridges and five breakdown analyses. Run in any order.
