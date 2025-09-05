> [!IMPORTANT]
> This repository is currently under construction!

# RSV Forecast Hub

This repository is designed to collect forecast data for the RSV Forecast Hub run by the US CDC. The project collects forecasts for two datasets:

1. Weekly new hospitalizations, due to RSV.
2. Weekly incident percentage of emergency department visits, due to RSV.

If you are interested in using these data for additional research or publications, please contact [rsvhub@cdc.gov](mailto:rsvhub@cdc.gov) for information regarding attribution of the source forecasts.

## Nowcasts And Forecasts Of Confirmed RSV Hospital Admissions

During the submission period, participating teams will be invited to submit national- and jurisdiction-specific (all 50 states, Washington DC, and Puerto Rico) probabilistic nowcasts and forecasts of the weekly number of confirmed RSV hospital admissions during the preceding [epidemiological week ("epiweek")](https://epiweeks.readthedocs.io/en/stable/background.html), the current epiweek, and the following three epiweeks.

The weekly total RSV admissions counts can be found in the `totalconfrsvnewadm` column of the [National Healthcare Safety Network](https://www.cdc.gov/nhsn/index.html) (NHSN) [Hospital Respiratory Data (HRD) dataset](https://www.cdc.gov/nhsn/psc/hospital-respiratory-reporting.html).

NHSN provides a preliminary release of each week's HRD data on Wednesdays [here](https://data.cdc.gov/Public-Health-Surveillance/Weekly-Hospital-Respiratory-Data-HRD-Metrics-by-Ju/mpgq-jmmr/about_data). Official weekly data is released on Fridays [here](https://data.cdc.gov/Public-Health-Surveillance/Weekly-Hospital-Respiratory-Data-HRD-Metrics-by-Ju/ua7e-t2fy/about_data). For more details on this dataset, its release schedule, and its schema, see the [NHSN Hospital Respiratory Data page](https://www.cdc.gov/nhsn/psc/hospital-respiratory-reporting.html).

## Nowcasts And Forecasts Of RSV Emergency Department Visits

The RSV Forecast Hub also accepts probabilistic nowcasts and forecasts of the proportion of emergency department visits due to RSV. This target represents RSV as a proportion of emergency department (ED) visits, aggregated by epiweek (Sunday-Saturday) and jurisdiction (states, DC, United States). The numerator is the number of visits with a discharge diagnosis of RSV, and the denominator is total visits. This target is optional for any submitted location and forecast horizon.

The weekly percent of ED visits due to RSV can be found in the `percent_visits_rsv` column of the [National Syndromic Surveillance Program](https://www.cdc.gov/nssp/index.html) (NSSP) [Emergency Department Visits - COVID-19, Flu, RSV, Sub-state](https://data.cdc.gov/Public-Health-Surveillance/NSSP-Emergency-Department-Visit-Trajectories-by-St/rdmq-nq56/about_data) dataset. Although these numbers are reported in the percentage form, we will accept forecasts as decimal proportions (i.e., `percent_visits_rsv / 100`). To obtain state-level data, we filter the dataset to include only the rows where the `county` column is equal to `All`.

This dataset will be updated on Wednesdays (preliminary release to facilitate forecasting) and Fridays (official weekly release) [here](https://data.cdc.gov/Public-Health-Surveillance/NSSP-Emergency-Department-Visit-Trajectories-by-St/rdmq-nq56/about_data).

The Wednesday data updates contain the same data that are published on Fridays at [NSSP Emergency Department Visit trajectories](https://data.cdc.gov/Public-Health-Surveillance/NSSP-Emergency-Department-Visit-Trajectories-by-St/rdmq-nq56/about_data) and underlie the percentage ED visit reported on the PRISM Data Channel's [Respiratory Activity Levels page](https://www.cdc.gov/respiratory-viruses/data/activity-levels.html), which is also refreshed every Friday. The data represent the information available as of Wednesday morning through the previous Saturday. For example, the most recent data available on the 2025-06-11 release (Wednesday) will be for the week ending 2025-06-07 (Saturday).

## Dates And Deadlines

The Challenge Period will begin September 17, 2025, and will run until May 2026.

Participants will be asked to submit nowcasts and forecasts by 11PM USA Eastern Time each Wednesday (the "Forecast Due Date"). If it becomes necessary to change the Forecast Due Date or time deadline, we will notify participants at least one week in advance.

Weekly submissions (including file names) will be specified in terms of a "reference date": the Saturday following the Forecast Due Date. This is the last day of the USA/CDC epiweek (Sunday to Saturday) that contains the Forecast Due Date.

## Prediction Targets And Horizons

Participating teams will be able to submit national- and jurisdiction-specific (all 50 states, Washington DC, and Puerto Rico) predictions for following targets.

### Targets

1. Quantile predictions for epiweekly total laboratory-confirmed RSV hospital admissions.
2. Individual forecast trajectories for epiweekly total laboratory-confirmed RSV hospitalizations over time (i.e sampled trajectories).
3. Quantile predictions for epiweekly percent of emergency department visits due to RSV.
4. Individual forecast trajectories for epiweekly percent of emergency department visits due to RSV over time (i.e sampled trajectories).

Targets 2, 3 and 4 are optional for any submitted location whereas target 1 (quantile predictions for epiweekly RSV hospital admissions) is mandatory for any submitted location and forecast horizon. Teams are encouraged but not required to submit forecasts for all weekly horizons or for all locations.

> [!NOTE]
>
> We are considering introducing samples-based ensembles for the hubs and would encourage teams to submit sample trajectories along with quantile forecasts.


### Horizons

Teams can submit nowcasts or forecasts for these targets for the following temporal "horizons":

- `horizon = -1`: the epiweek preceding the reference date
- `horizon = 0`: the current epiweek
- `horizon = 1, 2, 3`: each of the three upcoming epiweeks

### Epiweeks

We use epiweeks as defined by the [US CDC](https://wwwn.cdc.gov/nndss/document/MMWR_Week_overview.pdf), which run Sunday through Saturday. The `target_end_date` for a prediction is the Saturday that ends the epiweek of interest. That is:

```python
target_end_date = reference_date + (horizon * 7)
```

Standard software packages for R and Python can help you convert from dates to epiweeks and vice versa:

#### R

- [`lubridate`](https://lubridate.tidyverse.org/reference/week.html)
- [`MMWRweek`](https://cran.r-project.org/web/packages/MMWRweek/)

#### Python

- [`epiweeks`](https://pypi.org/project/epiweeks/)
- [`pymmwr`](https://pypi.org/project/pymmwr/)

## Further Submission Information

Detailed guidelines for formatting and submitting forecasts are available in the [`model-output` directory README](model-output/README.md). Detailed guidelines for formatting and submitting model metadata can be found in the [`model-metadata` directory README](model-metadata/README.md).

## Suggested Workflow For First Time Submitters

First-time pull requests (PRs) into the Hub repository must be reviewed and merged manually; subsequent ones can be merged automatically if they pass appropriate checks.

We suggest that teams submitting for the first time make a PR adding their model metadata file to the [`model-metadata` directory](model-metadata) by 4 PM USA Eastern Time on the Wednesday they plan to submit their first forecast. This will allow subsequent PRs that submit forecasts to be merged automatically, provided checks pass. We also request that teams sync their PR branch with the `main` branch using the `Update branch` button if their PR is behind the `main` branch, to ensure the automerge action runs smoothly.

## Alignment Between RSV Forecast Hub And Other Forecasting Hubs

We have made some changes from previous version of the [RSV Forecast Hub](https://github.com/HopkinsIDD/rsv-forecast-hub/tree/main) to align RSV forecasting challenges with COVID-19 forecasting via [COVID-19 Forecast Hub](https://github.com/CDCgov/covid19-forecast-hub/tree/main) and influenza forecasting run via the [Flusight Forecast Hub](https://github.com/cdcepi/FluSight-forecast-hub).

All Hubs will require quantile-based forecasts of epiweekly incident hospital admissions reported into NHSN, with the same -1:3 week horizon span. The COVIDhub and RSVhub optionally accept forecasts of proportion of Emergency department visits reported into NSSP. The Hubs also plan to share a forecast deadline of 11pm USA/Eastern time on Wednesdays.




## Accessing RSV Data On The Cloud

To ensure greater access to the data created by and submitted to this hub, real-time copies of files in the following directories are hosted on the Hubverse's Amazon Web Services (AWS) infrastructure, in a public S3 bucket: [bucket pending].

- `auxiliary-data`
- `hub-config`
- `model-metadata`
- `model-output`
- `target-data`

GitHub remains the primary interface for operating the RSV hub and collecting forecasts from modelers. However, the mirrors of hub files on S3 are the most convenient way to access hub data without using `git`/GitHub or cloning the entire hub to your local machine.

The sections below provide examples for accessing hub data on the cloud, depending on your goals and
preferred tools. The options include:

| Access Method              | Description                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| hubData (R)                | Hubverse R client and R code for accessing hub data.                                  |
| Pyarrow (Python)           | Python open-source library for data manipulation.                                     |
| AWS command line interface | Download data and use hubData, Pyarrow, or another tool for fast local access.        |

In general, accessing the data directly from S3 (instead of downloading it first) is more convenient. However, if performance is critical (for example, you're building an interactive visualization), or if you need to work offline,
we recommend downloading the data first.

<details markdown=1>

<summary>hubData (R)</summary>

[hubData](https://hubverse-org.github.io/hubData), the Hubverse R client, can create an interactive session for accessing, filtering, and transforming hub model output data stored in S3.

hubData is a good choice if you:

- already use R for data analysis
- want to interactively explore hub data from the cloud without downloading it
- want to save a subset of the hub's data (*e.g.*, forecasts for a specific date or target) to your local machine
- want to save hub data in a different file format (*e.g.*, `.parquet` to `.csv`)

### Installing hubData

To install `hubData` and its dependencies (including the `dplyr` and `arrow` packages), follow the [instructions in the hubData documentation](https://hubverse-org.github.io/hubData/#installation).

### Using hubData

hubData's [`connect_hub()` function](https://hubverse-org.github.io/hubData/reference/connect_hub.html) returns an [Arrow multi-file dataset](https://arrow.apache.org/docs/r/reference/Dataset.html) that represents a hub's model output data. The dataset can be filtered and transformed using dplyr and then materialized into a local data frame using the [`collect_hub()` function](https://hubverse-org.github.io/hubData/reference/collect_hub.html).

#### Accessing Model Output Data

Use hubData to connect to a hub on S3 and retrieve all model-output files into a local dataframe. (note: depending on the size of the hub, this operation will take a few minutes):

```r
library(dplyr)
library(hubData)

bucket_name <- "pending..."
hub_bucket <- s3_bucket(bucket_name)
hub_con <- hubData::connect_hub(hub_bucket, file_format = "parquet", skip_checks = TRUE)
model_output <- hub_con %>%
  hubData::collect_hub()

model_output
# pending...
```

Use hubData to connect to a hub on S3 and filter model output data before "collecting" it into a local dataframe:

```r
library(dplyr)
library(hubData)

bucket_name <- "pending..."
hub_bucket <- s3_bucket(bucket_name)
hub_con <- hubData::connect_hub(hub_bucket, file_format = "parquet", skip_checks = TRUE)
hub_con %>%
  dplyr::filter(target == "wk inc rsv hosp", location == "25", output_type == "quantile") %>%
  hubData::collect_hub() %>%
  dplyr::select(reference_date, model_id, target_end_date, location, output_type_id, value)


# pending...
```

- [Full hubData documentation](https://hubverse-org.github.io/hubData/)

</details>

<details markdown=1>

<summary>Pyarrow (Python)</summary>

Python users can use [Pyarrow](https://arrow.apache.org/docs/python/index.html) to work with hub data in S3.

Pandas users can access hub data as described below and then use the `to_pandas()` method to get a Pandas dataframe.

Pyarrow is a good choice if you:

- already use Python for data analysis
- want to interactively explore hub data from the cloud without downloading it
- want to save a subset of the hub's data (*e.g.*, forecasts for a specific date or target) to your local machine
- want to save hub data in a different file format (*e.g.*, `.parquet` to `.csv`)

### Installing Pyarrow

Use `pip` to install Pyarrow:

```sh
python -m pip install pyarrow
```

### Using Pyarrow

The examples below start by creating a Pyarrow dataset that references the hub files on S3.


#### Accessing Model Output Data

Get all model-output files. This example creates an in-memory [Pyarrow table](https://arrow.apache.org/docs/python/generated/pyarrow.Table.html) with all model-output files from the hub (it will take a few minutes to run).

```python
import pyarrow.dataset as ds
import pyarrow.fs as fs


# define an S3 filesystem with anonymous access (no credentials required)
s3 = fs.S3FileSystem(access_key=None, secret_key=None, anonymous=True)

# create a Pyarrow dataset that references the hub's model-output files
# and convert it to an in-memory Pyarrow table
mo = ds.dataset("rsv-forecast-hub/model-output/", filesystem=s3, format="parquet").to_table()

# to convert the Pyarrow table to a Pandas dataframe:
df = mo.to_pandas()
```

Get the model-output files for a specific team (all rounds).

```python
import pandas as pd
import pyarrow.dataset as ds
import pyarrow.fs as fs


# define an S3 filesystem with anonymous access (no credentials required)
s3 = fs.S3FileSystem(access_key=None, secret_key=None, anonymous=True)

# create a Pyarrow dataset that references a team's model-output files
# and convert it to an in-memory Pyarrow table
mo = ds.dataset("rsv-forecast-hub/model-output/pending.../", filesystem=s3, format="parquet").to_table()

# to convert the Pyarrow table to a Pandas dataframe:
df = mo.to_pandas()
df.head()

# pending...
```

- [Full documentation of Pyarrow table API](https://arrow.apache.org/docs/python/generated/pyarrow.Table.html)

Add a filter to model output data before converting it to a Pyarrow table. Filters are expressed as a [Pyarrow dataset Expression](https://arrow.apache.org/docs/python/generated/pyarrow.dataset.Expression.html#pyarrow.dataset.Expression).

```python
from datetime import datetime
import pandas as pd
import pyarrow as pa
import pyarrow.compute as pc
import pyarrow.dataset as ds
import pyarrow.fs as fs


# define an S3 filesystem with anonymous access (no credentials required)
s3 = fs.S3FileSystem(access_key=None, secret_key=None, anonymous=True)

# create a Pyarrow dataset that references a team's model-output files, apply filters,
# and convert it to an in-memory Pyarrow table
mo = ds.dataset("rsv-forecast-hub/model-output/", filesystem=s3, format="parquet").to_table(
  filter=(
    pc.field("target") == "wk inc rsv hosp") &
    pc.equal(pc.field("reference_date"), pa.scalar(datetime(2023, 10, 14), type=pa.date32())))

# to convert the Pyarrow table to a Pandas dataframe:
df = mo.to_pandas()
df.head()

# pending...
```

</details>


<details markdown=1>

<summary>AWS CLI</summary>

AWS provides a terminal-based command line interface (CLI) for exploring and downloading S3 files.

This option is ideal if you:

- plan to work with hub data offline but don't want to use git or GitHub
- want to download a subset of the data (instead of the entire hub)
- are using the data for an application that requires local storage or fast response times

### Installing AWS CLI

- Install the AWS CLI using the [instructions here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- You can skip the instructions for setting up security credentials, since Hubverse data is public

### Using AWS CLI

When using the AWS CLI, the `--no-sign-request` option is required, since it tells AWS to bypass a credential check
(*i.e.*, `--no-sign-request` allows anonymous access to public S3 data).

> [!NOTE]
>
> Files in the bucket's `raw` directory should not be used for analysis (they're for internal use only).

List all directories in the hub's S3 bucket:

```sh
aws s3 ls rsv-forecast-hub --no-sign-request
```

List all files in the hub's bucket:

```sh
aws s3 ls rsv-forecast-hub --recursive --no-sign-request
```

Download all of target-data contents to your current working directory:

```sh
aws s3 cp s3://rsv-forecast-hub/target-data/ . --recursive --no-sign-request
```

Download the model-output files for a specific team:

```sh
aws s3 cp s3://rsv-forecast-hub/model-output/pending/ . --recursive --no-sign-request
```

- [Full documentation for `aws s3 ls`](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)
- [Full documentation for `aws s3 cp`](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)

</details>





## Acknowledgments
This repository follows the guidelines and standards outlined by the [hubverse](https://hubdocs.readthedocs.io/en/latest/), which provides a set of data formats and open source tools for modeling hubs.


<details markdown=1>

<summary> CDC GitHub Guidelines </summary>

<br>


**General Disclaimer** This repository was created for use by CDC programs to collaborate on public health related projects in support of the [CDC mission](https://www.cdc.gov/about/cdc/#cdc_about_cio_mission-our-mission).  GitHub is not hosted by the CDC, but is a third party website used by CDC and its partners to share information and collaborate on software. CDC use of GitHub does not imply an endorsement of any one particular service, product, or enterprise.

## Related Documents

* [Open Practices](open_practices.md)
* [Rules of Behavior](rules_of_behavior.md)
* [Disclaimer](DISCLAIMER.md)
* [Contribution Notice](CONTRIBUTING.md)
* [Code of Conduct](code-of-conduct.md)


## Public Domain Standard Notice

This repository constitutes a work of the United States Government and is not subject to domestic copyright protection under 17 USC § 105. This repository is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/). All contributions to this repository will be released under the CC0 dedication. By submitting a pull request you are agreeing to comply with this waiver of copyright interest.

## License Standard Notice

The repository utilizes code licensed under the terms of the Apache Software License and therefore is licensed under ASL v2 or later.

This source code in this repository is free: you can redistribute it and/or modify it under the terms of the Apache Software License version 2, or (at your option) any later version.

This source code in this repository is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the Apache Software License for more details.

The source code forked from other open source projects will inherit its license.

## Privacy Standard Notice

This repository contains only non-sensitive, publicly available data and information. All material and community participation is covered by the [Disclaimer](DISCLAIMER.md) and [Code of Conduct](code-of-conduct.md). For more information about CDC's privacy policy, please visit [http://www.cdc.gov/other/privacy.html](https://www.cdc.gov/other/privacy.html).

## Contributing Standard Notice

Anyone is encouraged to contribute to the repository by [forking](https://help.github.com/articles/fork-a-repo) and submitting a pull request. (If you are new to GitHub, you might start with a [basic tutorial](https://help.github.com/articles/set-up-git).) By contributing to this project, you grant a world-wide, royalty-free, perpetual, irrevocable, non-exclusive, transferable license to all users under the terms of the [Apache Software License v2](http://www.apache.org/licenses/LICENSE-2.0.html) or later.

All comments, messages, pull requests, and other submissions received through CDC including this GitHub page may be subject to applicable federal law, including but not limited to the Federal Records Act, and may be archived. Learn more at [http://www.cdc.gov/other/privacy.html](http://www.cdc.gov/other/privacy.html).

## Records Management Standard Notice

This repository is not a source of government records, but is a copy to increase collaboration and collaborative potential. All government records will be published through the [CDC web site](http://www.cdc.gov).

</details>
