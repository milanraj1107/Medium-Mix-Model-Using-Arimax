# Medium-Mix-Model-Using-Arimax

## Introduction:
Sets the context by introducing the project topic, problem statement or opportunity addressed, project objectives, and the overall scope of the work.
Media Mix Modeling (MMM), also sometimes referred to as marketing mix modeling, is a statistical technique used in marketing to understand the impact of various marketing activities on a company's goals.
We planned to track down the impact of each channel spends on organic visits trends on the platform for the last 2 years. This analysis will help the company to optimise on the marketing spends on each channel. 

## Methodology:
Explains how you conducted the project. This includes research methods, data collection techniques, analytical tools used, and any specific procedures followed.
I employed ARIMAX time series forecasting model to work on the problem statement.
ARIMAX stands for AutoRegressive Integrated Moving Average with eXogenous variables. In simpler terms, ARIMAX is a time series forecasting model that forecasts the dependent variable based on the independent variables. 
In our problem statement, Organic Visits is the dependent variable and spends on each channel are the independent variable.
To validate the significance of each channel, I made various iterations. In each iteration, I removed one or more channels spends, and I ran ARIMAX model on each iteration. Post model training, ran predictions and evaluated the performance for each iteration. The evaluation metrics used are MAE ( mean absolute error), MSE ( mean squared absolute error) and RMSE ( root mean squared absolute error)

Iterations with comparatively lower numbers against evaluation metrics, highlights that particular channel/channels removed in that iteration is not significant as removing the component is improving the model prediction. 

## Peak Into the code:
<ul>
  <li>
    Data Collection: <br/>
    I took support from the marketing functional teams and analytics service tools ( FB ads  manager, GA, google ads manager) to fetch weekly spends on each channel for the last 2 years. 
  </li>
  <li>
    Data Cleaning
    <ul>
      <li>Replaced null values with 0</li>
      <li>Corrected the data types of columns ( date & float type)</li>
      <li>For any time-series forecasting on Python, the dataframes need to have dates as index and other variables as columns. Formatted the collected data in the required format.</li>
      <li><b>High dimensionality</b> - There was not much to do here. As in each iteration, we are any removing columns. Checked for correlation of each column with all other columns and removed one column form the combination that had more than 80% correlation. </li>
      <li>The dates need to sorted in the ascending order</li>
    </ul>
  </li>
  <li>
    Exploratory Data Analysis<br/>
    Independent variables contain spends on Meta, Google, ATL/BTL campaigns each divided at ad type. 
Meta & google columns consists of continuous data (leading to low variation)
ATL/BTL campaigns is surfacing as discrete data (leading to high variation)

ATL/BTL campaigns spends are revolving around event days. The spends on such campaigns are much higher than digital campaigns. On these event days, we have noticed a considerable jump in organic traffic as well. Given the nature of the data (mentioned above), there is a high probability that ARIMAX conclusion highlights major dependency of ATL/BTL campaigns. 

This brings us to the need that the data analysis will need to be done at a monthly and quarterly level as well where the ATL/BTL campaign spend will also come out as a continuous data. This should help us eliminate the shortcoming discussed above.
  </li>
  <li>
    Data Preparation
    <ul>
      <li><b>Stationary Data</b> - Stationary data refers to data where the statistical properties don't change over time.
To run time series forecasting, the dependent variable ned to stationary. Hence ran difference and log codes to make the data stationary.

Ran ADF and KPSS test to check the stationarity of the output data post running the code. 

Difference coded data surfaced to be the best possible choice
</li>
      <li><b>Iterations</b> – Group input variables based on the iteration type. Ex – one iteration is all data except Meta.</li>
      <li><b>Train & Test Data</b> – Divided each iteration and dependent variable into test and train data. (10% test and 90% train data) </li>
    </ul>
  </li>
  <li>
    Parameters:
    <ul>
      <li>ARIMAX model contains 3 parameters</li>
      <li>
        <ul>
          <li><b>Autoregressive (AR) Parameters (p)</b>: These parameters represent the influence of past values of the target variable itself on the current value. Ran PACF to find this value through visual inspection.</li>
          <li><b>Integrated (I) Parameter (d)</b>: This parameter reflects the degree of differencing needed to make the data stationary.</li>
          <li><b>Moving Average (MA) Parameters (q)</b>: These parameters account for the randomness or error terms in the data. Ran ACF to find this value through visual inspection.</li>
        </ul>
      </li>  
    </ul>
  </li>
  <li>
    Model training
    <ul>
      <li>Using the training data from each iteration and from dependent variable, I ran ARIMAX model for each iteration</li>
      <li>Then using the summary from the ARIMAX model for each iteration, ran predictions using test data. </li>
      <li>For each iteration, calculated MAE, MSE and RMSE./li>
      <li>Added the performance measure metrics output of each iteration in a dataframe format. </li>
    </ul>
  </li>
  <li>
    Results & Analysis <br/>
    Presents the findings of your project. Use data, tables, charts, and figures to illustrate your points. Analyze the results statistically if applicable, and explain their significance in relation to the project objectives
    Out of the four iterations, the best model is the first iteration type, i.e, the input data doesn't contains any columns with meta in it. 
This highlights meta campaigns to be the most insignificant. 
Iteration 2 suggests out of the digital budgets, more budget should be allocated to allocated to Google channels. 

Iteration 3 & 4, highlights the dependency of ATL/BTL activities is higher than digital campaigns with TV being a significant factor in ATL/BTL activities ( as the mae,mse and rmse numbers are further increasing)

  </li>
</ul>
