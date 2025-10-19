This project is split into 2 parts:

1. KS-PME analysis, Kaplan-Schoar (2005)
2. Valure creation analysis, Puche, Braun, Achleitner (2015)

1. KS-PME analysis

    Requirements:
    The analysis requires a "CoreData.xlsx", "currencies.csv" and "indices.csv", to be
    present in "InputData", and "deal_to_fund.csv" to be present in "GrossToNet/Data".
    Run in order:

    1. "GrossToNet/01GNDataPipeline.ipynb"
    This notebook handles data loading and cleaning for the PME analysis.
    - Ensure deal to fund mapping,
    - Drop unreasonable dates,
    - Ensure fund and deal coverage lies within benchmark and currency coverage
    - Ensure maximum one Fair Value/ NAV per Deal/ Fund as the terminal valuation
    Does not require changes from the user.

    2. "GrossToNet/02GrossToNetAnalysis.ipynb"
    This notebook handles:
    - Currency harmonization,
    - Composite Benchmark construction,
    - PME Engine,
    - PME analysis at pooled level and fund (gross and net) level.
    Does not require changes from the user.


2. Value Creation analysis

    Requirements:
    The analysis requires "CoreData.xlsx" and "InputData/monthly_interest_rates_curr.csv" to be present in "InputData".
    Run in order:

    1. "ValueCreation/01DataPipeline.ipynb"
    This notebook handles the data loading and working.csv construction
    - Loads deal timeseries data,
    - Enriches with deal data,
    - Enriches with fund data.
    Does not require changes from the user.

    2. "ValueCreation/02DataTransformation.ipynb"
    This notebook handles data cleaning and value creation input ratio computing.
    - Adds "holding_status",
    - Filters unreasonable dates,
    - Ensures clean EV-EqV bridge and >0 EBITDA,
    - Computes missing bridge items if 2/3 are available,
    - Matches Date-to-financial for exited and unexited deals and checks currency integrity,
    - Calculate value creation ratios (e.g. EBITDA multiple) for entry and exit and drop unreasonable metrics.

    Toggles for the user, cmd+F to find, () items are best as is and do not require changes:
        - ("ENFORCE_MIN_ROWS_PER_DEAL" option to exclude all deal_id's with less than 2 time series dates.)
        - "HOLDING_FILTER_MODE" option to run the analysis on all deals or exited_only deals.
        - ("REQUIRE_POSITIVE_EBITDA" option to also include deals with negative EBITDA.)
        - ("ENFORCE_POSITIVE_EV_AND_EQ" option to allow negative bridge items.)
        - ("Specify what deals will get dropped" option to customize cutoff for ratios.)

    3. "ValueCreation/03ValueCreationCalculationTotalSample.ipynb"
    This notebook handles the value bridge item computing
    - Collapses every deal to only entry and exit row (ensures 2 observations per deal),
    - Calculates TM levered, Leverage Effect and TM unlevered,
    - Calculates all other effects,
    - Computes all items in TM-Points,
    - Computes all items as share of times_money levered.

    Toggles for the user, cmd+F to find, () items are best as is and do not require changes:
        - ("APPLY_TM_SHORT_HOLD_FILTER" to allow for filtering of deals with short holding periods and near 0 times_money.)

    4. "ValueCreation/TotalSampleValueCreation" and "ValueCreation/ValueCreationBreakdown"
    These folders contain the visualization scrpits for the value creation bridges and 5 breakdown analyses. Run in any order.
