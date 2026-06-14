# AirQ Sensor Data Analytics
<img width="859" height="504" alt="image" src="https://github.com/user-attachments/assets/5e6bb458-902f-4f83-b1df-ead5a4e23658" />

AirQ Sensor Data Analytics is an academic data engineering and analytics project built around an air-Q air quality sensor.

The project demonstrates how environmental sensor data can be collected from a physical device, decoded, stored in a relational database and analyzed with Python-based data analysis workflows.

## Project overview

The goal of this project was to build a small end-to-end pipeline for air quality data:

1. Connect to an air-Q sensor
2. Retrieve encrypted sensor measurements
3. Decode and transform the raw measurements
4. Store the data in a relational database
5. Provide helper classes for database operations
6. Analyze and visualize environmental sensor data in Python

The project was developed as part of an academic / portfolio context and represents an early practical implementation of data ingestion, database handling and exploratory data analysis.

## Features

* Automated retrieval of air-Q sensor measurements
* AES-based decoding of encrypted sensor responses
* Storage of sensor measurements in MariaDB / MySQL
* Database table creation and record management
* Timestamp-based incremental data loading
* Object-oriented helper classes for database and analytics workflows
* Exploratory data analysis with pandas
* Correlation analysis and visualization
* Line charts, scatter plots and correlation heatmaps
* Documentation of sensor variables and known data anomalies

## Repository contents

| File                                           | Purpose                                                                                                      |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| `DBHandler.py`                                 | Handles air-Q sensor communication, message decoding, database connection, table creation and data insertion |
| `AnalyticsHandler.py`                          | Provides analysis and visualization functions for pandas DataFrames                                          |
| `MySQLManager.py`                              | Additional database management helper                                                                        |
| `LibraryImporter.py`                           | Helper for importing project libraries                                                                       |
| `Anleitung.ipynb`                              | Notebook-based usage guide                                                                                   |
| `Library_Anleitung.ipynb`                      | Notebook-based library guide                                                                                 |
| `DataDescription.txt`                          | Description of measured variables and known anomalies                                                        |
| `Dokumentation AirQ Sensor Data Analytics.pdf` | Project documentation                                                                                        |
| `Zwischenpräsentation.pptx`                    | Intermediate project presentation                                                                            |
| `Abschlusspräsentation.pptx`                   | Final project presentation                                                                                   |

## Data model

The database table is designed to store air quality and environmental measurements such as:

* particulate matter: `pm1`, `pm2_5`, `pm10`
* particle counts: `cnt0_3`, `cnt0_5`, `cnt1`, `cnt2_5`, `cnt5`, `cnt10`
* gases: `co`, `co2`, `no2`, `so2`, `o3`, `tvoc`
* climate data: `temperature`, `humidity`, `humidity_abs`, `dewpt`, `pressure`
* sound data: `sound`, `sound_max`
* derived indicators: `health`, `performance`, `dCO2dt`, `dHdt`
* timestamped measurement data

The `timestamp` field is used as the primary key for the measurement table.

## Technical stack

* Python
* pandas
* NumPy
* matplotlib
* seaborn
* MariaDB / MySQL
* SQLAlchemy
* Jupyter Notebook
* AES encryption / decryption for air-Q messages

## Architecture

```text
air-Q Sensor
    -> encrypted HTTP response
    -> Python decoding logic
    -> transformation into structured records
    -> MariaDB / MySQL database
    -> pandas DataFrame
    -> exploratory analysis and visualization
```

## Core components

### `DBHandler`

The `DBHandler` class is responsible for the operational part of the data pipeline.

It provides functionality for:

* connecting to the air-Q sensor
* encoding and decoding sensor requests and responses
* connecting to MariaDB / MySQL
* creating the target database table
* loading measurements for a given date range
* inserting new records into the database
* retrieving the latest stored timestamp
* deleting or dropping records and tables

### `AnalyticsHandler`

The `AnalyticsHandler` class provides exploratory analytics functionality on top of pandas DataFrames.

It includes functions for:

* triangular correlation heatmaps
* correlation analysis above a configurable threshold
* scatter matrix creation
* line chart visualization
* scatter plots with optional trend lines
* investigation of correlations over time
* rolling-window anomaly detection based on changing correlations
* comparison of correlations across multiple datasets

## Example workflow

A typical workflow looks like this:

```python
from DBHandler import DBHandler
from AnalyticsHandler import AnalyticsHandler

# Create database handler
db = DBHandler(
    airqIP="192.168.4.1",
    airqpass="airqsetup"
)

# Create table if needed
db.create_table("airq_measurements")

# Load sensor data into the database
db.airq_data_to_db(
    startdate="2023/04/01",
    enddate="2023/04/30",
    tablename="airq_measurements"
)

# Load data into a pandas DataFrame
# df = ...

# Analyze data
analytics = AnalyticsHandler(df)
analytics.triangle_correlation_heatmap()
analytics.analyze_correlations(threshold=0.7, plot=True)
```

## Known data anomalies

The dataset documentation contains known anomalies that should be considered during analysis:

* Measurement error related to fine dust values caused by a sudden increase in air humidity
* Lab power outage, probably caused by a short circuit
* Large fire near the measurement location

These events are relevant when interpreting outliers, sudden changes or unusual correlations in the sensor measurements.
<img width="989" height="639" alt="image" src="https://github.com/user-attachments/assets/67063393-340e-423b-b796-e777aae166ae" />


## Project status

This repository is a legacy academic / portfolio project.

It is kept public because it demonstrates early practical experience with:

* sensor data ingestion
* database-backed data collection
* Python-based data analysis
* object-oriented structuring of helper classes
* exploratory environmental data analytics

The project is not currently maintained as a production-ready package.


## Disclaimer

This project was created for learning and portfolio purposes. It is not intended to provide certified environmental monitoring or legally binding air quality assessment.
