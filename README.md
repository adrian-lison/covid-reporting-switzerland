# Reporting of Swiss COVID-19 data

This repository contains datasets that track the public reporting of Swiss COVID-19 data by the Federal Office of Public Health (FOPH) over time. It is based on snapshots of the public data from the [FOPH COVID-19 dashboard](https://www.covid19.admin.ch).

The datasets show how many cases that occurred on a certain day had already been reported on the same day, one day later, two days later, and so on. This is useful for empirical analyses of reporting delays and for nowcasting of epidemiological data. The data is available both for Switzerland as a whole and for each canton individually.

## Currently available datasets
- [Confirmed cases](data/reporting_cases.csv)
- [Hospitalizations](data/reporting_hospitalizations.csv)
- [Deaths](data/reporting_deaths.csv)

## Schema
Each dataset contains the following fields:
- `date_X` The date of occurrence of the corresponding event, where X is either case, hospitalizations, or death.
- `date_report` The date of public reporting by FOPH.
- `geoRegion` The code for the respective canton or all of Switzerland, see [here](https://www.covid19.admin.ch/api/data/documentation/models/sources-definitions-dailyincomingdata.md#georegion).
- `total` The cumulative number of events on `date_X` that have so far been reported until `date_report`. Ideally, this number should be strictly increasing over time. This is however not guaranteed, see [Caveats](#caveats).

## Coverage
The recording starts at `2020-11-05` (first available snapshot). The time series resolution is daily, but updates are not published by FOPH every day: Until `2022-04-01`, a daily reporting schedule except for weekends was used. After that, a weekly reporting schedule was used, with the new report being published every Tuesday.

## Caveats
Please be aware that since the publicly reported data is published by FOPH in an aggregated format, all corrections made to the data are subsumed in the cumulative count for a certain day. For example, if 100 new cases were reported for a certain day, but the dates of 10 already known cases for that day were changed to a different day as part of a correction, then the overall count will only increase by 90 cases for that day. Thus, the reporting delays as recorded by the aggregated data provided here may be slightly biased by data corrections. In extreme cases, this may even lead to *decreases* of the cumulative count, especially when the true number of new cases is rather low (e.g. small canton during low incidence time).

A particular anomaly to be aware of is in the hospitalization data from `2021-09-28` to (including) `2021-10-13`. Here, the cumulative count dropped significantly for many hospitalizations dates. The counts were corrected again on `2021-10-14`. It is therefore recommended to treat these data as missing and impute between `2021-09-27` and `2021-10-14` using some appropriate method.

## Contact
If you have questions about these data or how to use them, you can contact [me](https://bsse.ethz.ch/cevo/the-group/people/person-detail.html?persid=283358) via [email](mailto:adrian.lison@bsse.ethz.ch).
