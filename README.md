# Evaluating the success of Bradford 2025 City of Culture programme with Footfall 👣

## Abstract 🔎

Cultural events generate large volumes of data, yet the UK’s arts, culture and heritage sector continues to face fragmented, inconsistent and insufficiently granular data. These gaps hinder the sector’s ability to evidence its social, civic and economic value, thereby limiting policy development, investment, and alignment between local and national strategies. There is a need to unite various data sources for better cultural evaluation. Digital footprint data, particularly mobile app-derived footfall counts, offer a powerful additional data source to understand the impact of cultural activities, as footfall serves as a proxy indicator for town centre vitality and viability. This case study aims to evaluate the success of the Bradford City of Culture 2025 programme by investigating changes in footfall patterns across the Bradford district, assessing both immediate effects and potential longer-term legacy outcomes. 
Daily footfall count data derived from mobile applications will be combined with local weather conditions and temporal contextual factors (day of the week, bank holidays), to model pedestrian activity. A machine learning approach will be used to estimate footfall for 2025 in the absence of cultural events and compare these predictions with observed counts. Over- and under-predictions and their spatio-temporal dynamics will be used to track ‘excess’ moving populations, attributable to cultural activity. Additional forecast into 2026 will enable to explore post-event patterns.
The Bradford City of Culture 2025 programme was expected to generate measurable increases in footfall, reflecting its projected attraction of large visitor numbers and associated social and economic regeneration. The analysis anticipates identifying distinct spatial and temporal patterns of increased pedestrian activity during and after major events.
This research will demonstrate how mobile app digital footprints and predictive modelling can be leveraged to strengthen cultural impact assessment, supporting evidence-based decision-making for event organizers, funders and policy makers.

## Data 📊

The 'Data' folder contains all the raw datasets required for the cleaning and scraping steps, as well as the cleaned datasets required for modelling.
The data was received on the 5th of November 2025. **The datasets are not publically available and should not be shared.**

Different datasets were collected, cleaned and joined:

Variable | Unit | Description | Source
---------|------|----------|----------
Estimated Actual Footfall | | Number of visitors in area for a given day. | [HUQ](https://huq.io/insights/footfall-data/)
Estimated Actual Footfall Rolling | | Average number of visitors of the previous 6 days for the same day of the week in area | [HUQ](https://huq.io/insights/footfall-data/)
Cos_weekday_num| |Cosine of the week day number | 
Sin_weekday_num| |Sinus of the week day number | 
Cos_month_num| |  Cosine of the month number| 
Sin_month_num| | Sinus of the month number| 
Cos_week_of_year| | Cosine of the week of year number| 
Sin_week_of_year| | Sinus of the week of year number | 
Year| | | 
bank_hol| |Whether or not that day is a bank holiday (0= No, 1= Yes)|[Kaggle](https://www.kaggle.com/datasets/shivd24coder/uk-national-holidays-dataset)
Covid times| |Whether or not that day was during covid restrictions (0= No, 1= Yes)|
Precipitation | mm | Sum of daily precipitation (including rain, showers and snowfall) | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Temperature | °C | Average daily air temperature at 2 meters above ground | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Wind Speed | km/h | Maximum wind speed on a day | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)
Daylight Duration | seconds | Number of seconds of daylight per day | [Open Meteo API](https://open-meteo.com/en/docs/historical-weather-api?bounding_box=-90,-180,90,180&hourly=&daily=temperature_2m_mean,precipitation_sum,wind_speed_10m_max)

## Project Worflow ⚙️

### <ins> 1. Clean data </ins> ✨

This script involves cleaning the raw footfall data, creating contextual temporal variables, conducting exploratory data analysis and visualizations which can be used for future dashboards.

The ouput files from this is:
* `footfall_MetOf_Clean.csv`

### <ins> 2. Data Scraping </ins> ⛏️

This script involves adding more contextual variables by scraping data for weather (with API), UK bank holidays and covid times. The notebook ends by separating data for the 2019-2024 period and the 2025 period, and removing outliers in the 2019-2024 data using the Median Average Distance technique.

The ouput files from this are:
* `footfall_clean.csv (all years)`
* `footfall_19_24.csv (2019-2024)`
* `footfall_2025.csv (just 2025)`

### <ins> 3. Data Modelling </ins> 🤖

This notebook follows the data cleaning and scraping by modelling the footfall data. The model is selected, tuned and fitted using the footfall data between 2019 and 2024. The model is then used later on to predict the 2025 footfall, to allow event evaluation.

The below steps are followed:

#### 1) Model selection

The performance of four different machine learning models is tested using 10-fold cross validation. The models include:

* Linear regression
* Random Forest
* XGBoost
* Extra Trees Regressor

The outputs of the 10-fold cross validation process are used to calculate the error metric scores associated with that model (averaged over all folds). The MAE, the MAPE, the R2 and the RMSE metrics are compared to find the model that will best fit the data.

**Conclusion:** Random Forest Regression is the best performing model. After going through model selection **and** conducting model evaluation.

#### 2) Model Evaluation

The performance of the model is tested, using a 80-20 test split with the chronological order of the data preserved. The model performance is evaluated using the error metrics of MAE, MAPE, R2 and RMSE.

#### 3) Hyperparameter Tuning


Hyperparameter tuning is performed as it allows to find the best set of hyperparameters to maximise the model's efficiency and accuracy. 

#### 4) Fitting the Final Model

Using the optimal hyperparameters found during the tuning, the model is fitted again, this time using the whole dataset (no training and test splits).

#### 5) Feature Importance

The feature importance of the model predictor variables is investigated.

#### 6) Using Model to Evaluate Events (Recursive Forecasting)

The final model is used to quantify the change in footfall that would otherwise been predicted in 2025.

## Next Steps (After Xmas Break) 🔜

1) The issue so far is that the model has a quite low performance and tends to overestimate footfall. So far some of the temporal contextual variables were inputted using the cyclic encoding, thus I need to try using the one hot encoding technique instead to see if this improves model performance.
2) I need to add a 'was during Bradford 2025 program' variable, retrain and refit the model on the whole dataset (2019-2025), and then predict footfall in 2026 to forecast trends post Bradford25.
3) The whole analysis was done using the footfall data for the Met Office area (whole Bradford district), and should be repeated for other areas with daily footfall (example: BID area).

## Acknowledgments 🤝

The work, methodology and scripts of this project were mostly based on the following research:

Asher, M., Oswald, Y. and Malleson, N. (2025) ‘Understanding pedestrian dynamics using machine learning with real-time urban sensors’, Environment and Planning B: Urban Analytics and City Science, 52(8), pp. 1994–2017. doi:10.1177/23998083251319058. 
