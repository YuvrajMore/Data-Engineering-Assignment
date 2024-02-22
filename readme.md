# Data Engineering Assignment

## Steps involved:
## 1. Collecting from data source
## 2. Data Cleaning and Transformation
## 3. Converting transformed data to parquet and inserting into database.
## 4. Generating power BI report

### 1. Data Collection:
create_source_data.ipynb notebook uses yfinance and yahoo_fin libraries to fetch data of 10 equities. Data is collected directly in pandas dataframe and this data is saved in CSV and JSON formats. Data included is OHLC data, company name and sector, dividends and splits data. ETL_to_db.ipynb notebook reads the data from source and merges OHLC data into one dask dataframe. Splits and dividends data is read into pandas dataframes.

### 2. Data Cleaning and Transformation:
Data Cleaning steps mainly involve removing duplicate records, records with null values and outlier. Threshold is set for how many records can be removed, if the number of records to be dropped is too high, then exception is thrown instead of removing high number of records. Date datatype has to be kept consistend throughout the process as it can cause issues while being written to/from different formats. For date type consistency, Date column should be transformed as soon as data is imported.
Apart from reading from files, a short term analysis table is also created which contains analysis of OHLC data of past 56 days. Various indicators such as exponential moving average, bollinger bands, RSI etc are calculated using the ta-lib library. To get indicators of past 56 days, we need to import more than 56 records because some indicator calculations are performed on a window of records, so if window is n then first n records are null and need to be dropped. This also causes the index in parquet file to start from 15(for short term analysis file) as first 14 records are dropped.

### 3. Data storage:
Transformed data is then stored in corresponding database tables as well as parquet files inside the destination directory. Parquet is a file format optimized for frequent reading and analytical operation on data. It follows a columnar-hybrid approach to reduce time taken to read data and metadata is also stored within the file. Hence it is useful for analytical operations on data. Libraries used for inserting to postgresql databse is psycopg2. The database consists of 5 tables, equity, ohlc_feed, split_data, dividend_data and short_term_analysis. The equity table contains a primary key equity_id and a company_name columns, and the primary key is referenced by all other tables.

### 4. Report generation:
Power BI collects data from parquet files and some minor transformations are performed on data to get it ready for analysis. There are 3 dashboard in PBIX file, one for close and volume data, one for short term analysis and one for splits and dividends data. Visuals having date on x-axis appears to be aggregated as legend shows, but that is not the case, instead power bi provides date hierarchy feature, on use of which aggregated data will be shows to user. But when dates are preset without hierarchy, aggregations are not performed.

## Daily updation:
The create_source_data.ipynb notebook contains last two cells which will fetch 1 row of latest data from same libraries used for creation. This data is appended to existing files when the next cell is executed. In ETL_to_db, there is a cell which reads last row of data from all the files and inserts it to table in database. Once data source is created, these scripts can be run dialy to update the data inside the database table.

## Logging:
python logging library is used to create logger and log the process of source data creation and ETL. Two log files are created, which will log info upon succesful operation as well errors if encountered.
