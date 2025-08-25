# Target Data
This folder contains target data in [standard hubverse format](https://docs.hubverse.io/en/latest/user-guide/target-data.html), [`time-series.parquet`](time-series.parquet) with both RSV hospital admissions and emergency department visits data. `time-series.parquet` file is updated every Wednesday with the latest reported RSV hospital admissions and ED visits values. 
We will treat this data as the truth data for (eventual) evaluation of the forecasts submitted to the RSV Forecast Hub.

### Target Data Dictionary

The following columns are included in `time-series.parquet`:

| Column      | Description                                                        |
|-------------|--------------------------------------------------------------------|
| `date`      | Date of observation (YYYY-MM-DD)                                   |
| `location`  | Location code (2 digits FIPS or `"US"` for national data)          |
| `observation` | Numeric value for the target                                     |
| `as_of`     | Date the data was retrieved or processed                           |
| `target`    | Description of the metric (e.g., `"wk inc rsv prop ed visits"`) |



## Hospital Admissions Data

The hospital admission prediction target `wk inc rsv hosp` is the weekly number of confirmed RSV hospital admissions based on the `totalconfrsvnewadm` column of the [NHSN Hospital Respiratory Reporting](https://www.cdc.gov/nhsn/psc/hospital-respiratory-reporting.html). [Weekly official counts](https://data.cdc.gov/Public-Health-Surveillance/Weekly-Hospital-Respiratory-Data-HRD-Metrics-by-Ju/ua7e-t2fy/about_data) are publicly released on Fridays. [Preliminary counts](https://data.cdc.gov/Public-Health-Surveillance/Weekly-Hospital-Respiratory-Data-HRD-Metrics-by-Ju/mpgq-jmmr/about_data) are released on Wednesdays.

## Emergency Department Visits Data

The emergency department visits prediction target `wk inc rsv prop ed visits` is the weekly proportion of emergency department (ED) visits due to RSV based on [National Syndromic Surveillance Program](https://www.cdc.gov/nssp/index.html) (NSSP) [Emergency Department Visits - COVID-19, Flu, RSV, Sub-state](https://data.cdc.gov/Public-Health-Surveillance/NSSP-Emergency-Department-Visit-Trajectories-by-St/rdmq-nq56/about_data) dataset. Although these numbers are reported in the percentage form, we accept forecasts as decimal proportions. The target data values are therefore expressed that way in the [`time-series.parquet`](time-series.parquet) target data file.

The Wednesday release of NSSP [Emergency Department Visits - COVID-19, Flu, RSV, Sub-state](https://data.cdc.gov/Public-Health-Surveillance/NSSP-Emergency-Department-Visit-Trajectories-by-St/rdmq-nq56/about_data) dataset will be available around mid-July on data.cdc.gov. Until then, we will maintain a copy of the dataset and update it every Wednesday in the [`auxiliary-data/nssp-raw-data`](../auxiliary-data/nssp-raw-data) directory of our GitHub repository as a file named [`latest.csv`](../auxiliary-data/nssp-raw-data/latest.csv). In addition to this raw dataset, `wk inc rsv prop ed visits` is included in the [`time-series.parquet`](time-series.parquet) which is updated every Wednesday.
