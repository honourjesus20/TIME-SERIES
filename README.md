# TIME-SERIES

Building a time series model involves several steps, including data preparation, model selection, training, and evaluation. Additionally, visualization plays a crucial role in understanding the patterns and trends within the time series data. Here's a general outline of the process:

Import Libraries: Start by importing the necessary libraries, such as pandas, numpy, matplotlib, and scikit-learn.

Load and Preprocess Data: Load the time series data into a pandas DataFrame and preprocess it as needed. This may include handling missing values, converting data types, and ensuring the appropriate time index.

Visualize the Time Series: Plot the time series data to gain insights into its patterns, trends, and seasonality. You can use matplotlib or other visualization libraries to create line plots, scatter plots, histograms, etc.

Split the Data: Split the time series data into training and testing sets. Typically, the most recent data points are kept for testing.

Select a Model: Choose an appropriate time series model based on the characteristics of your data. Common models include ARIMA, SARIMA, Prophet, or deep learning models like LSTM or GRU.

Train the Model: Fit the selected model on the training data. Adjust the model parameters based on evaluation metrics and the observed performance.

Validate the Model: Apply the trained model on the testing set and evaluate its performance. Calculate metrics such as mean squared error (MSE), root mean squared error (RMSE), or mean absolute error (MAE) to assess the model's accuracy.

Visualize Predictions: Plot the actual values from the testing set along with the predicted values generated by the model. This visual representation helps you understand how well the model captures the underlying patterns and trends.

Fine-tune and Iterate: If the model's performance is not satisfactory, you can fine-tune the model by adjusting its parameters or try a different model altogether. Iterate this process until you achieve a satisfactory model.

It's important to note that the specific implementation details and steps may vary depending on the time series model you choose. Consider referring to the documentation and tutorials for the specific model you want to work with to ensure the correct implementation.

Stationarity
this is always in time series no matter how you avoid it,any time of statistical model applied the data should be stationary, if your time series data has a particular behaviour over time, there is a very high probability that it will follow the same in the future, non-stationarity in time series

trend(varying mean over time)
seasonality(variation of specific time frame)
Stationarity has constant mean acording to time, constant variance(distance from mean),a varaiance should be equal at the different time, autocovariance that does not depend on time (values at different time should have no correlation)
To check for stationarity

Rolling statistics(plot themoving average ormoving variance and see if it varies with time. more of a visual technique)
ADCF Test(null hypothesis is that the Ts is non-statinarity, the test results comprise of a test statistic and critical values and you can reject H0
#determining rolling statisticsfor checking stationarity

ADF (Augmented Dickey-Fuller) test is a statistical significance test which means the test will give results in hypothesis tests with null and alternative hypotheses. As a result, we will have a p-value from which we will need to make inferences about the time series, whether it is stationary or not.

Before going into the ADF test, we must know about the unit root test because the ADF test belongs to the unit root test.

Unit Root Test A unit root test tests whether a time series is not stationary and consists of a unit root in time series analysis. The presence of a unit root in time series defines the null hypothesis, and the alternative hypothesis defines time series as stationary.

Mathematically the unit root test can be represented as

Where,

Dt is the deterministic component. zt is the stochastic component. ɛt is the stationary error process. The unit root test’s basic concept is to determine whether the zt (stochastic component ) consists of a unit root or not.

There are various tests which include unit root tests.

Augmented Dickey-Fuller test. Phillips-perron test. KPSS test. ADF-GLS test Breusch-godfrey test. Ljung-Box test. Durbin-watson test. Let’s move into our motive, which is the Dickey-Fuller test.

Explanation of the Dickey-Fuller test. A simple AR model can be represented as:

where

yt is variable of interest at the time t ρ is a coefficient that defines the unit root ut is noise or can be considered as an error term. If ρ = 1, the unit root is present in a time series, and the time series is non-stationary.

If a regression model can be represented as

Where

Δ is a difference operator. ẟ = ρ-1 So here, if ρ = 1, which means we will get the differencing as the error term and if the coefficient has some values smaller than one or bigger than one, we will see the changes according to the past observation.

There can be three versions of the test.

test for a unit root test for a unit root with constant test for a unit root with the constant and deterministic trends with time So if a time series is non-stationary, it will tend to return an error term or a deterministic trend with the time values. If the series is stationary, then it will tend to return only an error term or deterministic trend. In a stationary time series, a large value tends to be followed by a small value, and a small value tends to be followed by a large value. And in a non-stationary time series the large and the small value will accrue with probabilities that do not depend on the current value of the time series.

The augmented dickey- fuller test is an extension of the dickey-fuller test, which removes autocorrelation from the series and then tests similar to the procedure of the dickey-fuller test.

The augmented dickey fuller test works on the statistic, which gives a negative number and rejection of the hypothesis depends on that negative number; the more negative magnitude of the number represents the confidence of presence of unit root at some level in the time series.

We apply ADF on a model, and it can be represented mathematically as

Where

ɑ is a constant ???? is the coefficient at time. p is the lag order of the autoregressive process. Here in the mathematical representation of ADF, we have added the differencing terms that make changes between ADF and the Dickey-Fuller test.

The unit root test is then carried out under the null hypothesis ???? = 0 against the alternative hypothesis of ???? < 0. Once a value for the test statistic.

it can be compared to the relevant critical value for the Dickey-Fuller test. The test has a specific distribution simply known as the Dickey–Fuller table for critical values.

A key point to remember here is: Since the null hypothesis assumes the presence of a unit root, the p-value obtained by the test should be less than the significance level (say 0.05) to reject the null hypothesis. Thereby, inferring that the series is stationary.

Implementation of ADF Test To perform the ADF test in any time series package, statsmodel provides the implementation function adfuller().

Function adfuller() provides the following information.

p-value Value of the test statistic Number of lags for testing consideration The critical values Next in the article, we will perform the ADF test with airline passengers data that is non-stationary, and temperature data that is stationary.

Importing the libraries:

from statsmodels.tsa.stattools import adfuller import pandas as pd import numpy as np Reading the airline-passengers data

path = '/content/drive/MyDrive/Yugesh/deseasonalizing time series/AirPassengers.csv' data = pd.read_csv(path, index_col='Month') Checking for some values of the data.

data.head()

Output:

Plotting the data.

data.plot(figsize=(14,8), title='alcohol data series')

Output:

Here we can see that the data we are using is non-stationary because the number of passengers is integrated positively with time.

Now that we have all the things we require, we can perform our test on the time series.

Taking out the passengers number as a series.

series = data['Passengers'].values series Output:

Performing the ADF test on the series:

ADF Test
result = adfuller(series, autolag='AIC') Extracting the values from the results:

print('ADF Statistic: %f' % result[0])

print('p-value: %f' % result[1])

print('Critical Values:')

for key, value in result[4].items(): print('\t%s: %.3f' % (key, value)) if result[0] < result[4]["5%"]: print ("Reject Ho - Time Series is Stationary") else: print ("Failed to Reject Ho - Time Series is Non-Stationary") Output:

Here in the results, we can see that the p-value for time series is greater than 0.05, and we can say we fail to reject the null hypothesis and the time series is non-stationary.

Now, let’s check the test for stationary data.

Loading the data.

path = '/content/drive/MyDrive/Yugesh/LSTM Univarient Single Step Style/temprature.xlsx' data = pd.read_excel(path, index_col='Date') Checking for some head values of the data:

data.head()

Output:

Here we can see that the data has the average temperature values for every day.

Plotting the data.

data.plot(figsize=(14,8), title='temperature data series')

Output:

Here we can see that in the data, the larger value follows the next smaller value throughout the time series, so we can say the time series is stationary and check it with the ADF test.

Extracting temperature in a series.

series = data['Temp'].values series Output:

Performing ADF test.

result = adfuller(series, autolag='AIC')

Checking the results:

print('ADF Statistic: %f' % result[0])

print('p-value: %f' % result[1])

print('Critical Values:')

for key, value in result[4].items(): print('\t%s: %.3f' % (key, value)) if result[0] > result[4]["5%"]: print ("Reject Ho - Time Series is Stationary") else: print ("Failed to Reject Ho - Time Series is Stationary") Output:

In the results, we can see that the p-value obtained from the test is less than 0.05 so we are going to reject the null hypothesis “Time series is stationary”, that means the time series is non-stationary.

In the article, we have seen why we need to perform the ADF test and the algorithms that the ADF and dickey-fuller test follow to make inferences about any time series. Statsmodel is one of the packages which allows us to perform many kinds of tests and analysis regarding time series analysis.

#perform dickey-fuller test
from statsmodels.tsa.stattools import adfuller

print('Results of Dickey-Fuller Test:')
dftest = adfuller(data['number_sold'], autolag='AIC')

dfoutput = pd.series(dftest[0:4], index = ['Test statistic','p-value','#lags used','number of obseravation used'])
for key,value in dftest[4].items():
    dfoutput['Critical Value (%s)'%key] =value
    
print(dfoutput)    
sns.lineplot(x= 'Date', y ='number_sold', color = 'red',data = data)
sns.lineplot(x= 'store', y ='number_sold', color = 'red',data = data)
###graphically interpretation intime series

trends occurs for a time the goes down like how things trend then die down
seasonality only happens at a particular time, a repeating pattern across years at a fixed time
irregularity follows no particular pattern happensforshort period like natural disaster something that only works based on the heat of time
cyclic is repeating over time with no pattern and harder to predict

chatgpt explanationary code Certainly! Here's a code template for building a time series model and visualizing the data using Python. This template demonstrates the steps mentioned earlier:

Explanation:

First, the necessary libraries are imported, including pandas, numpy, matplotlib, and the ARIMA model from statsmodels.

The time series data is loaded from a CSV file (replace 'time_series_data.csv' with the actual file name). You may need to preprocess the data here, such as handling missing values or converting data types.

The time series data is plotted using matplotlib to visualize its patterns and trends.

The data is split into training and testing sets using the train_test_split function from scikit-learn.

An appropriate model is selected. In this example, an ARIMA model with order (1, 0, 0) is used. You can adjust the model selection based on your specific needs.

The selected model is trained on the training data using the fit method.

The trained model is used to generate predictions on the testing set using the predict method.

The actual values and predicted values are plotted using matplotlib to visualize the model's performance.

Finally, you can fine-tune the model and iterate on the process as needed to improve its performance.

Remember to adapt this template to your specific dataset, model, and evaluation metrics as required.

Arima Model
AR+MA(two separate models binded together by integration) AR= Auto Regressive = P(auto regressive lags) correlation between previous time(p-1,p-2,p-3) and current time (p) I = Integration = d(order of differentiation) integrate p and q MA = Moving Average = Q(moving average)

plt.plot(data['store'], data['number_sol
