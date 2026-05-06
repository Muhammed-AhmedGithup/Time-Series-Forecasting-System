# Time Series Sales Forecasting Project

## Overview
This project focuses on forecasting sales using time series analysis techniques. It demonstrates a complete workflow from data loading and preprocessing to feature engineering, model training (specifically SARIMA), evaluation, and exposing predictions via a REST API using Flask. The goal is to predict future sales based on historical data, incorporating various time-based features and seasonal components.

## Features
-   **Data Loading & Preprocessing**: Reads sales data from an Excel file, handles data types, checks for duplicates and missing values, and aggregates data to a monthly frequency.
-   **Feature Engineering**: Creates relevant features for time series forecasting, including:
    -   Lag features (t-1, t-7, t-30)
    -   Rolling mean and standard deviation for 7 and 30-day windows
    -   Date-based features: Day of week, month, and year
    -   Holiday flag using the `holidays` library.
-   **Time Series Split**: Implements a robust training and validation split based on time series logic to prevent data leakage, reserving the last year of data for validation.
-   **SARIMA Model Implementation**: 
    -   Utilizes `pmdarima.auto_arima` to automatically select the best Seasonal AutoRegressive Integrated Moving Average (SARIMA) model parameters.
    -   Trains the SARIMA model on the aggregated monthly sales data.
    -   Generates forecasts for the validation period.
    -   Evaluates model performance using Root Mean Squared Error (RMSE) and Mean Absolute Error (MAE).
-   **API Exposure (Flask)**: A simple Flask API is set up to serve predictions from the trained SARIMA model. Users can query the API for future sales forecasts for a specified number of periods.

## Data
The project uses a dataset (e.g., `Forecasting Case- Study.xlsx`) containing sales information. Key columns include:
-   `State`: Geographical location (though currently aggregated).
-   `Date`: The date of the sales record.
-   `Total`: The total sales amount for that record.
-   `Category`: Product category (dropped during preprocessing as it had only one unique value).

## Models Implemented
### 1. SARIMA (Seasonal AutoRegressive Integrated Moving Average)
-   **Library**: `pmdarima` (a statistical library for Python).
-   **Methodology**: Automated parameter selection using `auto_arima` to find optimal (p,d,q)(P,D,Q,S) orders.
-   **Current Performance (on validation set)**:
    -   **SARIMA RMSE**: `{{rmse_sarima}}`
    -   **SARIMA MAE**: `{{mae_sarima}}`

### 2. Facebook Prophet (Planned)
-   The next step in this project is to implement and compare the forecasting performance of Facebook Prophet, a popular forecasting tool for business time series data.

## API Endpoint
The project includes a Flask application to serve SARIMA model predictions.

### Endpoint: `/predict`
-   **Method**: GET
-   **Query Parameters**:
    -   `periods`: An integer representing the number of future periods (months) for which to generate predictions.
-   **Example Request**:
    `YOUR_NGROK_URL/predict?periods=12`
-   **Example Response**:
    ```json
    [
        {
            "Date": "2023-01-01",
            "Predicted_Total": 43500070000.0
        },
        {
            "Date": "2023-02-01",
            "Predicted_Total": 45593280000.0
        }
        // ... more predictions
    ]
    ```

## Setup and Installation
To run this project, you'll need Python 3 and the following libraries:

```bash
pip install pandas openpyxl numpy matplotlib seaborn pmdarima flask holidays
```

## Usage
1.  **Clone the Repository (if applicable)**:
    ```bash
    git clone <your-repo-link>
    cd <your-repo-name>
    ```
2.  **Place Data**: Ensure your sales data file (`Forecasting Case- Study.xlsx`) is in the appropriate directory (e.g., `/content/`).
3.  **Run the Jupyter/Colab Notebook**: Execute the cells sequentially to perform data loading, preprocessing, feature engineering, model training, and evaluation.
4.  **Run the Flask API**: Once the SARIMA model (`stepwise_model`) is trained and the Flask app is defined, you can run the API. In a Colab environment, you typically use `ngrok` to expose your local Flask server.

    After executing the cell that defines `app`, execute `app.run()` in a new cell. If using `ngrok`, set it up as instructed in the notebook output to get your `YOUR_NGROK_URL`.

    ```python
    # In a new cell, after the API definition
    # !pip install flask_ngrok # if not already installed
    # from flask_ngrok import run_with_ngrok
    # run_with_ngrok(app)
    app.run()
    ```

5.  **Make Predictions**: Use the provided `YOUR_NGROK_URL` to send GET requests to the `/predict` endpoint as shown in the API Endpoint section.
