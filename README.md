# Datasets used in the CVGKAN load-forecasting study

This repository lists the datasets used in **“CVGKAN: A Hybrid Short-Term Load Forecasting Framework Based on Cascaded Signal Decomposition and GRU-KAN.”** The EECM-2016 Area 1 and BDG2 study files are in `data/`; source links for all five datasets are given below.

## Included study files

- [EECM-2016 Area 1 workbook](data/EECM-2016_Area1_2012-2013_corrected.xlsx): 70,176 rows at 15-minute intervals, SHA-256 `883ba483d4954ca1cc0a9365953650ab2607497374bed9477b048de0c92fc7f0`.
- [Processed BDG2 building series](data/Bear_education_Austin_processed.csv): 17,544 hourly rows, SHA-256 `a6821ee94a6c0fa251d6d5d34ab62aa9d44a4ab1700c3c10e499006448ff54c3`.

## Dataset overview

| Dataset | Period used | Original resolution | Forecast target | Role | Original source |
|---|---|---:|---|---|---|
| Panama | 3 Jan 2015, 01:00–3 Jan 2017, 23:00 | 1 h | National demand (MW) | Primary benchmark | [Mendeley Data, version 1](https://doi.org/10.17632/byx7sztj59.1) |
| EECM-2016 Area 1 | 1 Jan 2012–31 Dec 2013 | 15 min | Regional load (MW) | External benchmark | [2016 Problem A (organizer)](https://shumo.neepu.edu.cn/info/1121/1569.htm); [study workbook](data/EECM-2016_Area1_2012-2013_corrected.xlsx) |
| ISO-NE 2014 | 1 Jan–31 Dec 2014 | 1 h | System demand (MW) | External benchmark | [ISO New England load and demand reports](https://www.iso-ne.com/isoexpress/web/reports/load-and-demand) |
| Australian system load | Jan 2006–Dec 2009 | 30 min | System load (MW) | External benchmark | [Liu's Figshare dataset, version 2](https://doi.org/10.6084/m9.figshare.14958054.v2) |
| BDG2 building | 1 Jan 2016–31 Dec 2017 | 1 h | Building electricity (kWh/h) | External benchmark | [Processed study file](data/Bear_education_Austin_processed.csv); [Building Data Genome Project 2](https://github.com/buds-lab/building-data-genome-project-2); [dataset paper](https://doi.org/10.1038/s41597-020-00712-x) |

## Series and inputs used

### Panama

- Target: `nat_demand`.
- Inputs: historical demand; Santiago 2-m air temperature (`T2M_san`), 2-m specific humidity (`QV2M_san`), total precipitable liquid water (`TQL_san`), 2-m wind speed (`W2M_san`), and a holiday indicator.
- The selected period contains 17,567 consecutive hourly observations. The manuscript reports no missing values within this period.
- Source citation: Aguilar Madrid, E. *Short-term electricity load forecasting (Panama case study)*, Mendeley Data, version 1, 2021. DOI: [10.17632/byx7sztj59.1](https://doi.org/10.17632/byx7sztj59.1).

### EECM-2016 Area 1

- Target: regional electricity load.
- Inputs: daily mean temperature, relative humidity, and precipitation.
- The source sequence contains 70,176 observations at 15-minute intervals. The manuscript averages each consecutive group of four observations to form 17,544 hourly samples and replaces 96 missing precipitation records with 0 mm before hourly aggregation.
- Competition source: [2016 Problem A, “电力系统短期负荷预测” (organizer page)](https://shumo.neepu.edu.cn/info/1121/1569.htm). The workbook in this repository is the study copy.

### ISO-NE 2014

- Target: hourly New England system demand.
- Inputs: dry-bulb temperature and calendar variables (month, day, weekday, and hour).
- The selected 2014 period contains 8,760 hourly observations.
- Source: [ISO New England, Energy, Load, and Demand Reports](https://www.iso-ne.com/isoexpress/web/reports/load-and-demand).

### Australian system load

- Target: system load.
- Inputs: dry-bulb, dew-point, and wet-bulb temperatures; relative humidity; electricity price; and calendar variables.
- The source file contains 70,128 half-hourly observations. Consecutive pairs are averaged to produce 35,064 hourly samples.
- Source: Liu, R. *Austrial-load.xlsx* (filename as published), Figshare, version 2, 2021. DOI: [10.6084/m9.figshare.14958054.v2](https://doi.org/10.6084/m9.figshare.14958054.v2).

### BDG2 building

- The selected meter is `Bear_education_Austin`; “Austin” is an anonymized building identifier.
- Target: hourly building electricity consumption (kWh/h).
- Inputs: air and dew-point temperature, wind speed, sea-level pressure, cloud cover, precipitation, and calendar variables.
- The two-year timeline has 17,544 hourly timestamps. The manuscript reports 39 missing electricity readings. Training-set-derived hour-of-week medians, with fallbacks, impute missing historical inputs; timestamps with originally missing targets are excluded from fitting and evaluation.
- Original project: [Building Data Genome Project 2](https://github.com/buds-lab/building-data-genome-project-2). Dataset paper: Miller, C. et al. *Scientific Data* **7**, 368 (2020), DOI: [10.1038/s41597-020-00712-x](https://doi.org/10.1038/s41597-020-00712-x).

## Forecasting setup

- Samples are ordered chronologically and divided into training, validation, and test segments at 50%:10%:40%.
- Forecasting inputs use the preceding 24 hourly observations. Panama is evaluated at 1-, 12-, and 24-hour output settings; the four external datasets are evaluated one hour ahead.
- Normalization parameters and the sample-entropy clustering centers are fitted on training data.

## Reuse

Check the [BDG2 source licence](https://github.com/buds-lab/building-data-genome-project-2/blob/master/LICENSE) and the original providers’ terms before reusing these files.
