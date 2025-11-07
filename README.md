# Stock Price Viewer
## Project Overview

**Stock Price Viewer** is a FastAPI web application designed to fetch and visualize historical stock prices from Yahoo Finance.
The application is developed **with the help of AI-assisted programming using Gemini-pro-2.5,** which helped streamline the coding process and improve efficiency.

Given the following **required fields:**

- Stock Ticker
- Start Date
- End Date

The app will:

- Retrieve historical stock prices for the specified date range from Yahoo Finance.
- Generate a plot of stock price versus date for easy visualization.
- Display the most recent stock price, covering up to the last 20 days.

## Project Structures
  ```
    1 /home/henryjia/Projects/fastapi_stock_plot/
    2 ├── app/
    3 │   ├── __init__.py
    4 │   ├── main.py
    5 │   ├── static/
    6 │   │   └── style.css
    7 │   └── templates/
    8 │       ├── error.html
    9 │       ├── index.html
   10 │       └── result.html
   11 ├── tests/
   12 │   ├── __init__.py
   13 │   └── test_main.py
   14 ├── .gcloudignore
   15 ├── .gitignore
   16 ├── app.yaml
   17 ├── cloudbuild.yaml
   18 ├── README.md
   19 └── requirements.txt

  ```

## Running the Application Locally
  1. Activate the virtual environment 
  ```
  source venv/bin/activate
  ```
  2. Install the required dependencies
  ```
  pip install -r requirements.txt
  ```
  3. Start the FastAPI application
  ```
  uvicorn app.main:app --reload
  ```
  4. Open the app in your browser
  Visit [http://127.0.0.1:8000](http://127.0.0.1:8000) &rarr; to access the **Stock Price Viewer.**
     

## Unit Test in Local Enrionment
```
  pytest
```
 
  Custom Date Range Tests

  1. **test_plot**

   * Purpose: To verify that the primary plotting function works correctly when a user submits a ticker and a custom date range via
     the main form.
   * Action:
       1. The yfinance.download function is patched to return a series of pre-defined pandas DataFrames, simulating the download of
          stock data for the ticker, performance calculations, and market indices.
       2. A POST request is sent to the /plot endpoint with a mock ticker (AAPL) and a custom date range (2023-01-01 to 2023-01-03)
          using the TestClient.
   * Verification: Asserts that the HTTP status code of the response is 200, confirming that the request was processed successfully
     and a result page was generated.

  2. **test_plot_long_range**

   * Purpose: To ensure the plotting function can handle a larger dataset representing a longer date range without errors.
   * Action:
       1. yfinance.download is patched to return mock data for a 30-day period.
       2. A POST request is sent to the /plot endpoint with a mock ticker and a 30-day date range.
   * Verification: Asserts that the HTTP status code is 200, indicating the application successfully processed the larger dataset.

  3. **test_plot_no_data**

   * Purpose: To verify that the application gracefully handles cases where yfinance returns no data for a given ticker (e.g., an
     invalid ticker) by displaying the error page.
   * Action:
       1. yfinance.download is patched to return an empty DataFrame.
       2. A POST request is sent to the /plot endpoint with an 'INVALID' ticker.
   * Verification:
       1. Asserts that the HTTP status code is 200.
       2. Asserts that the response text contains the expected title (<title>Error</title>) and content of the error page.

  4. **test_plot_ytd**

   * Purpose: To verify that the endpoint for generating a "Year to Date" (YTD) plot works correctly.
   * Action:
       1. yfinance.download is patched to return a minimal mock DataFrame.
       2. A GET request is sent to the /plot/AAPL/YTD endpoint.
   * Verification: Asserts that the HTTP status code is 200.

  5. **test_plot_1y**

   * Purpose: To verify that the endpoint for generating a "1 Year" plot works correctly.
   * Action:
       1. yfinance.download is patched to return a minimal mock DataFrame.
       2. A GET request is sent to the /plot/AAPL/1y endpoint.
   * Verification: Asserts that the HTTP status code is 200.

  6. **test_plot_5y**

   * Purpose: To verify that the endpoint for generating a "5 Year" plot works correctly.
   * Action:
       1. yfinance.download is patched to return a minimal mock DataFrame.
       2. A GET request is sent to the /plot/AAPL/5y endpoint.
   * Verification: Asserts that the HTTP status code is 200.

  7. **test_plot_max**

   * Purpose: To verify that the endpoint for generating a "MAX" (maximum available data) plot works correctly.
   * Action:
       1. yfinance.download is patched to return a minimal mock DataFrame.
       2. A GET request is sent to the /plot/AAPL/MAX endpoint.
   * Verification: Asserts that the HTTP status code is 200.


## Deploment To GCP 
```
  gcloud builds submit --config cloudbuild.yaml .
```


## Running the Application on GCP

  Open the app in the broser: [https://fastapi-stock-plot.uc.r.appspot.com/](https://fastapi-stock-plot.uc.r.appspot.com/)
