

> [!IMPORTANT]
> This repository is currently under construction!

# *[Insert title of hub]*

*[Describe overall purpose of hub.]* Anyone interested in using these data for additional research or publications is
requested to contact *[insert email]* for information regarding attribution of the source forecasts.

## *[Insert title of forecasts]*

*[Describe forecasts]*

**Dates:** The Challenge Period will begin *[insert start date]* and will run until *[insert start date]*. Participants
are currently asked to submit *[insert description of forecasts]*  by *[insert timing]* .(herein referred to as the
Forecast Due Date). In the event that timelines of data availability change, *[insert name of hub]*  may change the
day of week that forecasts are due. In this case, participants would be notified at least one week in advance.
*[insert temporal period]* submissions (including file names) will be specified in terms of the reference date, which
is the Saturday following the Forecast Due Date. The reference date is the last day of the epidemiological week (EW)
(Sunday to Saturday) containing the Forecast Due Date.

**Prediction Targets:**
Participating teams are asked to provide *[insert geographical requirements]* predictions for targets
*[insert name of target(s)]*.

Teams will submit *[insert description of forecasts]* for the epidemiological week (EW) ending on the reference date
as well as *[insert horizons]*. Teams can but are not required to submit forecasts for all *[insert temporal period]*
horizons or for all locations. The evaluation data for forecasts will be the *[insert temporal period]* aggregate of
*[insert description of evaluation data]*. We will use the specification of EWs defined by the
[CDC](https://wwwn.cdc.gov/nndss/document/MMWR_Week_overview.pdf), which run Sunday through Saturday. The target end
date for a prediction is the Saturday that ends an EW of interest, and can be calculated using the expression:
**target end date = reference date + horizon * (*[insert # days in temporal period]* days)**.

There are standard software packages to convert from dates to epidemic weeks and vice versa (*e.g.*,
[MMWRweek](https://cran.r-project.org/web/packages/MMWRweek/) and
[lubridate](https://lubridate.tidyverse.org/reference/week.html) for R and [pymmwr](https://pypi.org/project/pymmwr/)
and [epiweeks](https://pypi.org/project/epiweeks/) for Python).

If you have questions aboutthis target, please reach out to *[insert name]* (*[insert email]*).

## Accessing hub data on the cloud

*[Remove this section if you're not planning to host your hub's data on the Hubverse's AWS infrastructure]*

To ensure greater access to the data created by and submitted to this hub, real-time copies of its model-output,
target, and configuration files are hosted on the Hubverse's Amazon Web Services (AWS) infrastructure,
in a public S3 bucket:
*[insert S3 bucket name, as specified in admin.json cloud.host.storage_location]*

**Note**: For efficient storage, all model-output files in S3 are stored in parquet format, even if the original
versions in the GitHub repository are .csv.

GitHub remains the primary interface for operating the hub and collecting forecasts from modelers.
However, the mirrors of hub files on S3 are the most convenient way to access hub data without using git/GitHub or
cloning the entire hub to your local machine.

The sections below provide examples for accessing hub data on the cloud, depending on your goals and
preferred tools. The options include:

| Access Method              | Description                                                                           |
| -------------------------- | ------------------------------------------------------------------------------------- |
| hubData (R)                | Hubverse R client and R code for accessing hub data                                   |
| Polars (Python)            | Python open-source library for data manipulation                                      |
| AWS command line interface | Download hub data to your machine and use hubData or Polars for local access          |

In general, accessing the data directly from S3 (instead of downloading it first) is more convenient. However, if
performance is critical (for example, you're building an interactive visualization), or if you need to work offline,
we recommend downloading the data first.

<!-------------------------------------------------- hubData ------------------------------------------------------->

<details>

<summary>hubData (R)</summary>

[hubData](https://hubverse-org.github.io/hubData), the Hubverse R client, can create an interactive session
for accessing, filtering, and transforming hub model output data stored in S3.

hubData is a good choice if you:

- already use R for data analysis
- want to interactively explore hub data from the cloud without downloading it
- want to save a subset of the hub's data (*e.g.*, forecasts for a specific date or target) to your local machine
- want to save hub data in a different file format (*e.g.*, parquet to .csv)

### Installing hubData

To install hubData and its dependencies (including the dplyr and arrow packages), follow the [instructions in the hubData documentation](https://hubverse-org.github.io/hubData/#installation).

### Using hubData

hubData's [`connect_hub()` function](https://hubverse-org.github.io/hubData/reference/connect_hub.html) returns an [Arrow
multi-file dataset](https://arrow.apache.org/docs/r/reference/Dataset.html) that represents a hub's model output data.
The dataset can be filtered and transformed using dplyr and then materialized into a local data frame
using the [`collect_hub()` function](https://hubverse-org.github.io/hubData/reference/collect_hub.html).


#### Accessing target data

*[hubData will be updated to access target data once the Hubverse target data standards are finalized.]*

#### Accessing model output data

Below is an example of using hubData to connect to a hub on S3 and filter the model output data.

```r
library(dplyr)
library(hubData)

bucket_name <- "hub-bucket-name"
hub_bucket <- s3_bucket(bucket_name)
hub_con <- hubData::connect_hub(hub_bucket, file_format = "parquet", skip_checks = TRUE)
hub_con %>%
  dplyr::filter(location == "MA", output_type == "quantile") %>%
  hubData::collect_hub()

```

- [full hubData documentation](https://hubverse-org.github.io/hubData/)

</details>

<!--------------------------------------------------- Polars ------------------------------------------------------->

<details>

<summary>Polars (Python)</summary>

The Hubverse team is currently developing a Python client (hubDataPy). Until hubDataPy is ready,
the [Polars](https://pola.rs/) library is a good option for working with hub data in S3.
Similar to pandas, Polars is based on dataframes and series. However, Polars has a more straightforward API and is
designed to work with larger-than-memory datasets.

Pandas users can access hub data as described below and then use the `to_pandas()` method to convert a Polars dataframe
to pandas format.

Polars is a good choice if you:

- already use Python for data analysis
- want to interactively explore hub data from the cloud without downloading it
- want to save a subset of the hub's data (*e.g.*, forecasts for a specific date or target) to your local machine
- want to save hub data in a different file format (*e.g.*, parquet to .csv)

### Installing polars

Use pip to install Polars:

```sh
pip install polars
```

### Using Polars

The examples below use the Polars
[`scan_parquet()` function](https://docs.pola.rs/api/python/dev/reference/api/polars.scan_parquet.html), which returns a
[LazyFrame](https://docs.pola.rs/api/python/stable/reference/lazyframe/index.html).
LazyFrames do not perform computations until necessary, so any filtering and transforms you apply to the data are
deferred until an explicit
[`collect()` operation](https://docs.pola.rs/api/python/stable/reference/lazyframe/api/polars.LazyFrame.collect.html#polars.LazyFrame.collect).

#### Accessing target data

Get all oracle-output files into a single DataFrame.

```python
import polars as pl

oracle_data = pl.scan_parquet(
    # the structure of the s3 link below will depend on how your hub organizes target data
    "s3://[hub-bucket-name]/target-data/oracle-output/*/*.parquet",
    storage_options={"skip_signature": "true"}
)

# filter and transform as needed and collect into a dataframe, for example:
oracle_dataframe = oracle_data.filter(pl.col("location") == "MA").collect()
```

#### Accessing model output data

Get the model-output files for a specific team (all rounds).
This example uses
[glob patterns to read from data multiple files into a single dataset](https://docs.pola.rs/user-guide/io/multiple/#reading-into-a-single-dataframe).

```python
import polars as pl

lf = pl.scan_parquet(
    "s3://[hub-bucket-name]/model-output/[modeling team name]/*.parquet",
    storage_options={"skip_signature": "true"}
)
```

#### Using partitions (hive-style)

If your data uses hive-style partitioning, Polars can use the partitions to filter the data before reading it.

```python
from datetime import datetime
import polars as pl

oracle_data = pl.scan_parquet(
    "s3://[hub-bucket-name]/target-data/oracle-output/",
    hive_partitioning=True,
    storage_options={"skip_signature": "true"}) \
.filter(pl.col("nowcast_date") == datetime(2025, 2, 5)) \
.collect()
```

- [Full documentation of the Polars Python API](https://docs.pola.rs/api/python/stable/reference/)

</details>

<!--------------------------------------------------- AWS CLI ------------------------------------------------------->

<details>

<summary>AWS CLI</summary>

AWS provides a terminal-based command line interface (CLI) for exploring and downloading S3 files.
This option is ideal if you:

- plan to work with hub data offline but don't want to use git or GitHub
- want to download a subset of the data (instead of the entire hub)
- are using the data for an application that requires local storage or fast response times

### Installing the AWS CLI

- Install the AWS CLI using the
[instructions here](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- You can skip the instructions for setting up security credentials, since Hubverse data is public

### Using the AWS CLI

When using the AWS CLI, the `--no-sign-request` option is required, since it tells AWS to bypass a credential check
(*i.e.*, `--no-sign-request` allows anonymous access to public S3 data).

> [!NOTE]
> Files in the bucket's `raw` directory should not be used for analysis (they're for internal use only).

List all directories in the hub's S3 bucket:

```sh
aws s3 ls [hub-bucket-name] --no-sign-request
```

List all files in the hub's bucket:

```sh
aws s3 ls [hub-bucket-name] --recursive --no-sign-request
```

Download all of target-data contents to your current working directory:

```sh
aws s3 cp s3://[hub-bucket-name]/target-data/ . --recursive --no-sign-request
```

Download the model-output files for a specific team:

```sh
aws s3 cp s3://[hub-bucket-name]/[modeling-team-name]/UMass-flusion/ . --recursive --no-sign-request
```

- [Full documentation for `aws s3 ls`](https://docs.aws.amazon.com/cli/latest/reference/s3/ls.html)
- [Full documentation for `aws s3 cp`](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)

</details>

## Acknowledgments

This repository follows the guidelines and standards outlined by [the
[hubverse](https://hubverse.io), which provides a set of data formats and open source tools for modeling hubs.



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
