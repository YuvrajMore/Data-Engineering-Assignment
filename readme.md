# Data Engineering Assignment

## Steps involved:
## 1. Collecting from data source
## 2. Data Cleaning and Transformation
## 3. Converting transformed data to parquet file format and inserting to database.
## 4. Generating power BI report

### 1. Data Collection:
create_source_data.ipynb notebook uses yfinance and yahoo_fin libraries to fetch data of 10 equities. Data is collected directly in pandas dataframe and this data is saved in CSV and JSON formats. Data included is OHLC data, company name and sector, dividends and splits data.

### 2. Data Cleaning and Transformation:
ETL_to_db.ipynb notebook read the data from source and merges ohlc data into one dask dataframe. splits and dividends data is read into pandas dataframes. The data is cleaned - removing outliers, null values, duplicate records etc. Short term analysis table is also creaeted which contains analysis of past 56 days is performed on data to get various features such as exponential moving average, bollinger bands, RSI etc.

### 3. Data storage:
Transformed data is then stored in corresponding database table as well as parquet files, which are optimized for frequent reads and calculations. Libraries used for inserting to postgresql databse is psycopg2.

### 4. Report generation:
Power BI collects data from parquet files and some minor transformations are performed on data to get it ready for analysis. There are 3 dashboard in PBIX file, one for close and volume data, one for short term analysis and one for splits and dividends data.

## Daily updation:
The create_source_data.ipynb notebook contains last two cells which will fetch 1 row of latest data from same libraries. This data is appended to existing files. In ETL_to_db, there is a cell which reads last row of data from all the files and inserts it to table in database. Once data source is created, these scripts can be run dialy to update the data inside the database table.

## Logging:
python logging library is used to create logger and log the process of source data creation and ETL. 
