# Machine Learning Inspired Wind Energy Forecast
#### Capstone project for The Data Incubator program
#### Andrew LaCroix<br><br>


### 1. Business Objective:
The overall goal of this project is to provide accurate, long-term predictions of wind-energy output from wind farms. Currently, wind energy forecasts rely on computationally intensive [numerical weather prediction models](https://en.wikipedia.org/wiki/Numerical_weather_prediction), which provide reliable estimates of wind energy production up to a few hours in advance. These "now-cast" models are used primarily for energy grid balancing, ensuring the amount of energy produced equals the amount consumed by customers. However, the accuracy of these models declines after about 48hrs. An accurate forecast that could be made further in advance would provide a number of competitive advantages for companies managing wind farm energy resources:

**A. Knowledge of when to store and/or sell surplus energy -** Minimize the number of times energy is purchased from other providers, and maximize the number of times excess energy is available to sell. This can maximize revenue made through energy trading.
   
**B. Optimize timing of maintenance -** Schedule maintenance of wind turbines when predicted energy output will be low.
   
**C. Enable deployment of more renewable systems -** Due to their inherent variability, variable renewable sources such as wind and solar will never account for 100% of the total energy produced. Excess power will need to be stored when the wind doesn't blow or the sun doesn't shine, most promisingly in storage systems like [pumped hydroelectric power plants](https://en.wikipedia.org/wiki/Pumped-storage_hydroelectricity). Long-term energy forecasts will aid in deciding when to store energy in pumped-hydro plants (which take hours to days to "charge"), enabling a greater percentage of the total energy consumed to come from renewable resources.<br><br>


### 2. Data Injestion:
 - Hourly wind farm energy production data from 2015-2018 in `./wind_power_csv/` came from the [IESO](http://www.ieso.ca/en/Power-Data/Data-Directory) (Independent Electricity System Operator) of Ontario, Canada and can be found [here](http://reports.ieso.ca/public/GenOutputCapability/PUB_GenOutputCapability.xml) in `.xml` format. Wind farm locations were manually estimated based on this [system map](http://www.ieso.ca/localContent/ontarioenergymap/index.html), saved as `ieso_locations.csv`. These data reflect hourly measurements of wind energy production from 25 distinct wind farms and were relatively small, totalling ~50 MB data.
 - Hourly temperature, wind speed, and wind direction data from individual weather stations is collected by National Oceanographic and Atmospheric Association (NOAA) and was accessed via NOAA's [FTP server](ftp.ncdc.noaa.gov/pub/data/noaa/). Data was combined from all weather stations in Ontario, Canada from 2015-2018. In total, the maximum amount of data injested summed to ~0.5 GB, from 153 weather stations yielding > 5,000,000 rows of measurements.
 - **Note:** Cleaning, time-synchronizing, and joining these datasets was accomplished using `pandas DataFrames`<br><br>

### 3. Visualizations:
Several visualizations were made using a combination of Python's `matplotlib` and `seaborn` libraries. The majority of these visualizations were made interactive via `ipywidgets` `interact` function. Here is a list of the most prominent visualizations:

**A. Distance between wind farms and weather stations -** 1D strip-plot showing that weather stations and wind farms were close enough in proximity to analyze together.

**B. Seasonality in wind energy and weather data -** Line plot showing daily and yearly seasonality trends in wind power output, indicating it is important to include hour-of-the-day and month-of-the-year as predictors in time-series modeling. Note: these seasonalities were also reflected in wind speed data, since it is the strongest predictor of wind energy generation.

**C. Autocorrelation in wind energy output -** Line plot showing that wind energy autocorrelations quickly drop to zero off after ~5hrs, indicating that naive wind energy forecasts (future energy output = current energy output) would only be reliable up to ~2.5hrs in the future.

**D. Time-series LSTM model optimization -** Several line plots showing the progression of model optimization/development:
 - addition of more hidden layers did not increase the predictive power of the model
 - dropout layers increased robustness of model to overfitting, but increased training time (n_epochs required to reach minimal loss)
 - regularization also lead to more robust models, with very small alpha
 - learning rates larger (faster) than the default 0.001 actually led to models with higher accuracy
 
**E. Model predictions vs naive model -** Line plots and box plots showing LSTM model outperforms naiive predictions across all timescales, especially for predictions made > 48 hrs in advance.

**F. Quality of predictions of final model -** Scatter plot showing that, up to 1 week in advance, cumulative energy generated over a 24hr period tracks closely with the actual energy generated.<br><br>


### 4. Machine Learning
The most critical tool that led to the success of this proposal was the use of **deep learning**, specifically a recurrent neural network designed to detect patterns for **time-series analysis**. This model, termed long short-term memory (LSTM), provided long-term (>1 week), accurate predictions of wind power output based on several input datasources (previous power output, current weather conditions, and temporal information like time of day and month of the year).<br><br>


### 5. Deliverable
For simplicity, all data injestion, cleaning, modeling, and visualizations have been combined into a single jupyter notebook `lacroix-capstone-wind-energy-forecast.ipynb`.
