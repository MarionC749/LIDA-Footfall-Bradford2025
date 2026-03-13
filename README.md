# Evaluating the impact of cultural events using machine learning and digital footfall: a case study of Bradford City of Culture 2025. 👣

## Abstract 🔎

Cultural events are hugely important, yet the UK’s arts, culture and heritage sector struggles to evidence its social, civic and economic value due to fragmented, inconsistent and insufficiently granular data. These gaps hinder the sector’s ability to develop policy, attract investment, and align local and national strategies, pointing to a need to unite various data sources to better understand the impacts of cultural events. Digital footprint data can fill these gaps. 

Footfall data derived from digital footprints, in particular, can support our understanding of the impact of cultural activities on neighbourhood vitality. Footfall, i.e. a measure of the number of visitors to places, serves as a proxy indicator for town centre vitality and viability. This case study aims to evaluate the success of the Bradford City of Culture 2025 programme by investigating changes in footfall patterns across the Bradford district, assessing both immediate effects and potential longer-term legacy outcomes.

Daily footfall count data derived from mobile applications will be combined with local weather conditions and temporal contextual factors (day of the week, bank holidays, etc.), to model pedestrian activity. A machine learning approach will be used to estimate footfall during 2025 in the absence of cultural events and compare these predictions with observed counts. Over- and under-predictions and their spatio-temporal dynamics will be used to track ‘excess’ moving populations, attributable to cultural activity after taking all other important drivers into account. Additional forecast into 2026 will enable to explore post-event patterns.
The Bradford City of Culture 2025 programme was expected to generate measurable increases in footfall, reflecting its projected attraction of large visitor numbers and associated social and economic regeneration. The analysis anticipates identifying distinct spatial and temporal patterns of increased pedestrian activity during and after major events.

This research will demonstrate how mobile app digital footprints and predictive modelling can be leveraged to strengthen cultural impact assessment, supporting evidence-based decision-making for event organizers, funders and policy makers.

## Run Instructions 💻

Acquire the datasets stored in the `Data` folder.
1. Run the `1- Exploratory Data Analysis.ipynb`, to explore the data.
2. Run `2- Footfall-Cleaning.ipynb`
3. Run `3- Data Scraping.ipynb`
4. Run `4- Modelling.ipynb`
5. Run `5- 2026 Modelling.ipynb`
6. For more information, explore `Other Notebooks` folder.

## Data 📊

The `Data` folder contains all the raw datasets required for the cleaning and scraping steps. The HUQ data was received on the 5th of November 2025, and the Kaggle data was accessed on the 9th of December 2025. **The HUQ datasets are not publically available and thus not shared here.**

Different datasets were collected, cleaned and joined to have the following variables ready for modelling:

Variable | Unit | Description | Source
---------|------|----------|----------
Estimated Actual Footfall | | Number of visitors in area for a given day. | [HUQ](https://huq.io/insights/footfall-data/)
Cos_weekday_num| |Cosine of the week day number | 
Sin_weekday_num| |Sinus of the week day number | 
Cos_month_num| |  Cosine of the month number| 
Sin_month_num| | Sinus of the month number| 
Cos_week_of_year| | Cosine of the week of year number| 
Sin_week_of_year| | Sinus of the week of year number | 
is_weekend | | Whether or not that day is a weekend (0= No, 1= Yes)|
bank_hol| |Whether or not that day is a bank holiday (0= No, 1= Yes)|[Kaggle](https://www.kaggle.com/datasets/shivd24coder/uk-national-holidays-dataset)
school_hol| |Whether or not that day is a school holiday (0= No, 1= Yes)|[Bradford Council](https://bradford.gov.uk/)
Covid times| |Whether or not that day was during covid restrictions (0= No, 1= Yes)|
Precipitation | mm | Sum of daily precipitation (including rain, showers and snowfall) | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Temperature | °C | Average daily air temperature at 2 meters above ground | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Wind Speed | km/h | Maximum wind speed on a day | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Daylight Duration | seconds | Number of seconds of daylight per day | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)

The **Bradford locations** used for modelling were:
*    'Bradford - BID'
*    'Bradford - City Centre'
*    'Bradford - Lister Park'
*    'Bradford - Roberts Park'
*    'Bradford - Local Authority'

During data exploration, geographical data on green spaces (specifically Roberts Park geometry) was also collected from the [Ordnance Survey](https://www.ordnancesurvey.co.uk/products/os-open-greenspace).

## Project Worflow ⚙️

### <ins> 1. Exploratory Data Analysis </ins> 📊

This notebook involves cleaning the raw footfall data and conducting exploratory data analysis, as well as creating visualizations which can be used for a dashboard.

The ouput files from this are:
* `footfall_map.html`
* `footfall_plots.html`

### <ins> 2. Clean data </ins> ✨

This notebook involves cleaning the raw footfall data and creating contextual temporal variables.

The ouput files from this are:
* `missing_days_counts.csv`
* `footfall_Mix_Clean.csv`

### <ins> 3. Data Scraping </ins> ⛏️

This notebook involves adding more contextual variables by scraping data for weather (with API), UK bank holidays, school holidays and covid times. The notebook ends by separating data for the 2019-2024 period and the 2025 period, and removing outliers in the 2019-2024 data using the Median Average Distance technique.

The ouput files from this are:
* `footfall_clean.csv` (all years)
* `footfall_19_24.csv` (2019-2024)
* `footfall_2025.csv` (just 2025)

### <ins> 4. Data Modelling </ins> 🤖

This notebook is the continuity of the data cleaning and scraping by modelling the footfall data. The model is selected, tuned and fitted using the footfall data between 2019 and 2024. The model is then used later on to predict the 2025 footfall, to allow Bradford City of Culture programem impact evaluation.

The below steps are followed:

#### 1) Model selection

The performance of four different machine learning models is tested using 10-fold cross validation. The models include:

* Linear regression
* Random Forest
* XGBoost
* Extra Trees Regressor

The outputs of the 10-fold cross validation with TimeSeriesSplit process are used to calculate the error metric scores associated with that model (averaged over all folds). The MAE, the MAPE, the R2 and the RMSE metrics are compared to find the model that will best fit the data.

**Conclusion:** Random Forest Regression is the best performing model.

#### 2) Model Evaluation

The performance of the Random Forest Regression model is tested, using a 80-20 test split with the chronological order of the data preserved using TimeSeriesSplit. The model performance is evaluated using the error metrics of MAE, MAPE, R2 and RMSE.

#### 3) Hyperparameter Tuning

Hyperparameter tuning is performed as it allows to find the best set of hyperparameters to maximise the model's efficiency and accuracy. 

#### 4) Fitting the Final Model

Using the optimal hyperparameters found during the tuning, the model is fitted again, this time using the whole dataset (no training and test splits).

#### 5) Feature Importance

The feature importance of the model predictor variables is investigated.

#### 6) Cross-Validated SHAP for Feature Importance

The feature importance of the model predictor variables is investigated using SHAP.

#### 7) Using Model Forecast to Evaluate Events

The final model is used to quantify the change in footfall that would otherwise been predicted in 2025.


## Other Work Included ℹ️

The `Other Notebooks` folder includes other notebooks which were not part of the final analysis but contributed to the project work.
* `Footfall Insights` folder contains 3 notebooks which were built to create various insights for different stakeholders (NCDO and Bradford 2025)
* `SARIMAX Attempt` folder contains the notebooks used to try building a SARIMA model to predict footfall in 2025. The analysis was not used in the end as the performance of the Random Forest Regression was better and more appropriate to incorporate footfall from different locations.


## Acknowledgments 🤝

The work, methodology and scripts of this project were mostly based on the following research:

Asher, M., Oswald, Y. and Malleson, N. (2025) ‘Understanding pedestrian dynamics using machine learning with real-time urban sensors’, Environment and Planning B: Urban Analytics and City Science, 52(8), pp. 1994–2017. doi:10.1177/23998083251319058. 
