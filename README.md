# Predict-Power
The main objective of this project is to develop a predictive model for power consumption based on time series weather data

Link to Jupyter Notebook:
https://github.com/saeedjazebi/Predict-Power/blob/main/CapStone.ipynb

Disclaimer:
in Module 16, I have aimed to use a different dataset and methodology for EV charging infrastructure siting applications. However, I could not find the appropriate public dataset for that project and so I decided to define a new project with a new scope, as below.

Data Source:
https://www.kaggle.com/datasets/fedesoriano/electric-power-consumption

Project Objectives:
The main objective of this project is to develop a predictive model for power consumption based on time series weather data (temperature, humidity, wind speed, and diffuse flow). The data is collected from Supervisory Control and Data Acquisition System (SCADA) of Amendis which is a public service utility provider and in charge of the distribution of water and electricity since 2002, in Morocco. The dataset is exhaustive in its demonstration of energy consumption of the Tétouan city in Morocco. The city is fed by three main distribution substations, that each serve a particular zone, namely: Quads, Smir, and Boussafou. 
The objective is to predict energy consumption in advance to inform electricity prices in a dynamic electricity market. This data could also be used to support data-driven decision making. 

Data Set Description
The data consists of 52,416 observations of energy consumption on 10-minute intervals (6 samples per hour). Every observation is described by 9 feature columns. The data is collected for a full year.
1.	Date Time: Time window of ten minutes (Input Feature)
2.	Temperature: Weather Temperature (Input Feature)
3.	Humidity: Weather Humidity (Input Feature)
4.	Wind Speed: Wind Speed (Input Feature)
5.	General Diffuse Flows (Input Feature)
6.	Diffuse Flows (Input Feature)
7.	Zone 1 Power Consumption (Target Feature)
8.	Zone 2 Power Consumption (Target Feature)
9.	Zone 3 Power Consumption (Target Feature)

Exploratory Data Analysis
Started with importing the required libraries.
Read the data from csv files.
Explore the data, feature names, and type of the data. The data is all numerical. Only Datetime feature need to be converted to proper format.
Explored missing values, but the data set does not have missing values.
Checked the statistics of all data fields to achive a better understanding. 
Checked the correlation between different variables. The Temperature and power consumption have highest positive correlation. Humidity has a negative correlation with temperature and power consumption. Wind speed increases the power consumption generally. DiffuseFlow has generally lower correlation with other parameters, specially it seems to have no correlation with target variables (power consumption).
Checked the distribution of data via histograms. No data anomaly observed.
Used Pairplot to visualize relationships between numerical columns.
Convert the 'Datetime' column to datetime format.
Plotted the energy consumption for the three zones on the same plot and compared the time-series behavior between the three zones.
Normalized all columns except 'Datetime' to be able to plot all important variables on the same plot. Plotted the Power consumption for each zone with temperature, humidity and windspeed to visualize the trends between the variables. Generally temperature follows the same trend as the power consumption.
Looking at the distribution of the data it does not seem that we have a lot of outliers in the data. Since the data is time-series data, if the outliers are removed, they should be imputed by mean, mode, or their next data sample, which might skew the data in other ways, so decided not to remove the outliers for this trial.
No duplicate rows were found in the dataset.
I am planning to eventually use three methods for this project:
1-	Predictive Regression: Predict power consumption for three different regions based on Temperature, Humidity, WindSpeed, GeneralDiffuseFlows, and DiffuseFlows. For this method, the time component could be dropped from the data.
2-	Time-series Autoregression, ARIMA method.
3-  Deep learning with Neural Networks.

In this preliminary EDA study, I focus on one regression method only for method 1 above.  

Used Sequential Feature Selection (SFS) method to identify which parameters are most important and critical to predict power consumption in Zone 1, 2, and 3. The Temperature, Humidity, and WindSpeed seem to be the most critical features. Hence, I used these three features as input variables.

Tested both Ridge and Lasso regressions. Used GridSearchCV to optimize both. Used the root mean square error and the r2 score to assess the performance of the regression models. The Ridge regression is outperforming. 

    	Training RMSE	Testing RMSE	Training R^2	Testing R^2
Ridge	    0.86	            0.86	        0.25       	0.25
Lasso	    0.88	            0.88         	0.21	      0.22

Also, Lasso revealed that for each zone different combinations of features provide the best performance, by selecting those features:
Zone 1	---> Temperature	// Temperature*Humidity	// Humidity^2
Zone 2	---> Temperature	// Temperature*WindSpeed	// Humidity^2
Zone 3	---> Temperature^2	// Temperature*WindSpeed
