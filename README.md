# Out For How Long? Predicting the Severity of Planned Power Shutoffs

In recent years, California has seen an uptick in wildfire frequency and severity as a result of climate change. Pacific Gas & Electric (PG&E) has had many lawsuits filed against them because of the role their power lines play in contributing to wildfire events. As a result, PG&E began to implement Public Safety Power Shutoff (PSPS) events to reduce the likelihood of their starting or contributing to wildfire events. These events, however, disrupt lives. We want to predict the severity of these events (with  shutoff length as a proxy) based on weather conditions (e.g., temperature, wind), geographic location, and census data (e.g., population, income) given that PG&E planned a shutoff.

To construct the models, you can run the files in the `code` folder in numeric order (e.g., `1-*`, `2-*`, ...). Note that files with the same initial number can be run in any order/concurrently. Also keep in mind that the weather scraper (https://github.com/ethan-allavarpu/cs229-pge-final-project/blob/main/data/weather/2b_scrape_weather.ipynb) is in the data directory and numbered as `2b-` to gather daily weather data, so it should happen before `3-` (adding the weather data to the main data frame) should be run. Note that we manually imputed some latitude and longitude. To run the scripts after we data clean and perform manual imputation (i.e., starting with `3-`), you can use the following data file: https://github.com/ethan-allavarpu/cs229-pge-final-project/blob/main/data/processed/processed-shutoffs.csv . To start with fully cleaned and merged data (so even with the weather data and start at the train/test split, `4-`), use the following data: https://github.com/ethan-allavarpu/cs229-pge-final-project/blob/main/data/processed/processed-shutoffs-weather.csv .

## Required Libraries

- Python: `copy`, `datetime`, `geopy`, `glob`, `fuzzywuzzy`, `matplotlib`, `meteostat`, `numpy`, `optuna`, `os`, `pandas`, `re`, `seaborn`, `sklearn`, `torch`, `warnings`, `xgboost`

- R: `maps`, `maptools`, `sp`, `tidyverse`

## Data Links

- Note that the census data is too large to house in the repository
  - Population and Demographics: <https://data.census.gov/cedsci/table?q=zcta&tid=ACSDP5Y2020.DP05>
  - Income: <https://data.census.gov/cedsci/table?q=income%20by%20zcta&tid=ACSST5Y2020.S1901>
- Substation Locations: <https://cecgis-caenergy.opendata.arcgis.com/datasets/7f37f2535d3144e898a53b9385737ee0_0/explore?location=39.055127%2C-122.040695%2C11.00>
- Weather Data: https://meteostat.net/en/place/us/stanford?s=KPAO0&t=2022-09-21/2022-09-28
- PG&E Reports (PDFs): https://www.pge.com/en_US/residential/outages/public-safety-power-shuttoff/psps-reports.page
- ZIP-to-ZCTA Converter: https://udsmapper.org/zip-code-to-zcta-crosswalk/

## Model Performance


| Model                     |Test R-Squared|RMSE           |MAE            |
|:--------------------------|-------------:|--------------:|--------------:|
|Simple Linear Regression   |   0.002391   |  1578.099447  |  1157.988578  |
|Multiple Linear Regression |   0.465563   |  1155.053925  |   882.595626  |
|XGBoost (Preliminary)      |   0.576786   |  1027.860530  |   750.430526  |
|Ridge                      |   0.496509   |  1121.114728  |   854.537449  |
|LASSO                      |   0.498145   |  1119.291615  |   852.692513  |
|Elastic Net                |   0.490046   |  1128.287164  |   860.886639  |
|**Random Forest**          | **0.779628** | **741.706667**| **487.372001**|
|KNN                        |   0.731378   |   818.888812  |   537.441052  |
|**XGBoost**                | **0.784097** | **734.146636**| **497.710402**|
|Neural Network             |   0.708749   |   852.683707  |   600.820038  |

The best model(s) is (are) listed in **bold**.

The models are listed in the order in which they were created (earliest to latest).
