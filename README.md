# Time-Series-Forecasting-Stock-Price-Forecasting-Using-Facebook-Prophet
Time Series Forecasting: Stock Price Forecasting Using Facebook Prophet
Certainly! Let's break down the code into step-by-step explanations:

### Step 1: Understanding Facebook Prophet

In the introductory section, we provide an overview of Facebook Prophet, highlighting its significance in forecasting time series data, especially in the context of stock prices. We emphasize its unique capabilities, such as handling missing data, outliers, and holidays, which make it a versatile tool for financial data analysis.

### Step 2: Getting Started

#### Substep 2.1: Installing and Importing Libraries

The first practical step involves installing the Prophet library and importing the necessary Python libraries. This includes pandas for data manipulation, Prophet for time series forecasting, and yfinance for fetching historical stock data from Yahoo Finance.

```python
# Libraries
# Install the Prophet library using pip
!pip install prophet

# Import necessary libraries
import pandas as pd
from prophet import Prophet
from datetime import datetime

# Ignore any warning messages
import warnings
warnings.filterwarnings("ignore")

# Import yfinance library and override its pandas_datareader
# This is done to enable the download of historical stock data
import yfinance as yf
yf.pdr_override()
```

#### Substep 2.2: Downloading Historical Stock Data

Next, we define the stock symbol of interest (e.g., 'AMD') and specify the start and end dates for fetching historical data. The code then uses the yfinance library to download the historical stock data and displays the first few rows of the dataset.

```python
# Define the stock symbol
stock = 'AMD'  # Input the stock symbol of interest

# Define the start and end dates for fetching historical data
start = '2020-01-01'  # Input the start date for historical data
end = date.today()     # Use the current date as the end date

# Download historical stock data using Yahoo Finance
df = yf.download(stock, start, end)

# Display the first few rows of the downloaded data
print(df.head())
```

#### Substep 2.3: Visualizing Historical Stock Prices

This substep involves plotting the historical stock prices to visually inspect the trends. The matplotlib library is used for plotting.

```python
# Visualize the historical stock prices
plt.figure(figsize=(16, 8))
plt.plot(df['Adj Close'])  # Plotting the 'Adj Close' prices
plt.title('Stock Price Over Time')  # Setting the title of the plot
plt.xlabel('Date')  # Setting the label for the x-axis
plt.ylabel('Price')  # Setting the label for the y-axis
plt.show()  # Display the plot
```

#### Substep 2.4: Preparing Data for Prophet

The code resets the index to make 'Date' a regular column, selects the 'Date' and 'Close' columns, and renames them to 'ds' and 'y', respectively, to make the data compatible with Prophet.

```python
# Resetting index to make 'Date' a regular column
df = df.reset_index()

# Selecting the 'Date' (ds) and 'Close' (y) columns from the stock_data DataFrame
df = df[['Date', 'Close']]

# Renaming columns for compatibility with Prophet
# 'Date' is renamed to 'ds' (datestamp), and 'Close' is renamed to 'y' (target variable)
df = df.rename(columns={'Date': 'ds', 'Close': 'y'})
```

### Step 3: Creating and Training the Prophet Model

#### Substep 3.1: Instantiating the Prophet Model

The code instantiates a Prophet model.

```python
# Instantiate Prophet model
m = Prophet()
```

#### Substep 3.2: Fitting the Model with Historical Stock Data

The model is fitted with the historical stock data.

```python
# Fit the model with historical stock data
m.fit(df)  # The model is trained using the historical stock data
```

### Step 4: Making Predictions

#### Substep 4.1: Creating a DataFrame with Future Dates

A DataFrame with future dates is created using the `make_future_dataframe` method of the Prophet model.

```python
# Create a dataframe with future dates for prediction
# The `periods` parameter determines how many future data points to create
future = m.make_future_dataframe(periods=365)  # Generating 365 days (1 year) of future dates
```

#### Substep 4.2: Generating Predictions

Predictions for the future dates are generated using the `predict` method.

```python
# Generate predictions for the future dates using the fitted model
forecast = m.predict(future)
```

### Step 5: Visualizing the Forecast

#### Substep 5.1: Plotting the Forecast

The forecasted stock prices are visualized using the `plot` method.

```python
# Visualize the forecast
fig1 = m.plot(forecast)

# Add title to the plot
plt.title(f'{stock} Stock Price Forecast with Prophet')

# Label the x-axis
plt.xlabel('Date')

# Label the y-axis
plt.ylabel('Stock Price (USD)')

# Display the plot
plt.show()
```

#### Substep 5.2: Visualizing Components of the Forecast

Components of the forecast, including trends, weekly patterns, and yearly patterns, are visualized using the `plot_components` method.

```python
# Visualize components of the forecast
fig2 = m.plot_components(forecast)

plt.show()
```

By following these steps, users can leverage Facebook Prophet to forecast stock prices and gain insights into future trends.
