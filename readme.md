# DAT535 - Temperature Forecasting
This project implements a temperature forecasting pipeline using deep learning and machine learning models in a distributed Spark environment. The project includes data ingestion, data cleaning and data serving using Convolutional Neural Network (CNN), Linear Regression, and Long Short Term Memory (LSTM).

## Pipeline Components
The pipeline is structured according to the Medallion Architecture with three layers: Data Ingestion, Data Cleaning, and Data Serving. All layers can be executed from the main.py file.
### Data Ingestion
The data ingestion component fetches the temperature readings from the Frost API. The data is fetched from Asker from 2000.01.01 to 2024-10-18 . The data is then unstructed and saved as a text file in HDFS.

### Data Cleaning
The data cleaning component reads raw data from HDFS, extracts relevant information, and calculates daily average temperatures from hourly readings. The cleaned data is then saved as a Parquet file in HDFS.

### Data Serving
The project uses three models to forecast the temperature. The models are CNN, Linear Regression, and LSTM. The models are trained and evaluated using various performance metrics.

## Running the Pipeline
1. **Set up Spark**: Install and configure Spark on your local machine or cluster.
2. **Set up HDFS**: Install and configure HDFS on your local machine or cluster.
3. **Sign up for Frost API**: Sign up for the Frost API and get an API key. Save in a .env file in the root directory with the following format:
```
<USERNAME>
<PASSWORD>
``` 
Frost API link: https://frost.met.no/index.html

4. **Run the pipeline**: Run the main.py file to run all pipeline components sequentially.
