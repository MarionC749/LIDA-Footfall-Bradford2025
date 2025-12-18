# Evaluating the success of Bradford 2025 City of Culture programme with Footfall

## Abstract:

Cultural events generate large volumes of data, yet the UK’s arts, culture and heritage sector continues to face fragmented, inconsistent and insufficiently granular data. These gaps hinder the sector’s ability to evidence its social, civic and economic value, thereby limiting policy development, investment, and alignment between local and national strategies. There is a need to unite various data sources for better cultural evaluation. Digital footprint data, particularly mobile app-derived footfall counts, offer a powerful additional data source to understand the impact of cultural activities, as footfall serves as a proxy indicator for town centre vitality and viability. This case study aims to evaluate the success of the Bradford City of Culture 2025 programme by investigating changes in footfall patterns across the Bradford district, assessing both immediate effects and potential longer-term legacy outcomes. 
Daily footfall count data derived from mobile applications will be combined with local weather conditions and temporal contextual factors (day of the week, bank holidays), to model pedestrian activity. A machine learning approach will be used to estimate footfall for 2025 in the absence of cultural events and compare these predictions with observed counts. Over- and under-predictions and their spatio-temporal dynamics will be used to track ‘excess’ moving populations, attributable to cultural activity. Additional forecast into 2026 will enable to explore post-event patterns.
The Bradford City of Culture 2025 programme was expected to generate measurable increases in footfall, reflecting its projected attraction of large visitor numbers and associated social and economic regeneration. The analysis anticipates identifying distinct spatial and temporal patterns of increased pedestrian activity during and after major events.
This research will demonstrate how mobile app digital footprints and predictive modelling can be leveraged to strengthen cultural impact assessment, supporting evidence-based decision-making for event organizers, funders and policy makers.

## Data

The 'Data' folder contains all the raw datasets required for the cleaning and scraping steps, as well as the cleaned datasets required for modelling.



## Project Worflow

### <ins> 1. Clean data </ins> 

This script involves cleaning the raw footfall data, creating contextual temporal variables, conducting exploratory data analysis and visualizations which can be used for future dashboards.

The ouput files from this is:
* footfall_MetOf_Clean.csv

### <ins> 2. Data Scraping </ins> 

This script involves adding more contextual variables by scraping data for weather (with API), UK bank holidays and covid times. The notebook ends by separating data for the 2019-2024 period and the 2025 period, and removing outliers in the 2019-2024 data using the Median Average Distance technique.

The ouput files from this are:
* footfall_clean.csv (all years)
* footfall_19_24.csv (2019-2024)
* footfall_2025.csv (just 2025)

### <ins> 3. Data Modelling </ins> 

