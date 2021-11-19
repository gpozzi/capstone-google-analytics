# Capstone Project: NYC Green Taxis

# Summary / Scope

<img src="https://user-images.githubusercontent.com/52865532/142680713-61a35034-0413-46e4-ac08-d4c38299c54e.jpg" width="500">

In this page, I'll be documenting a comprehensive analysis of a fictitious business case. This was done as a capstone project for my [Google Data Analytics Professional Certificate](https://www.credly.com/earner/earned/share/8c3b6269-5e44-40d0-be37-e3f9c8e8f3da) and will include the use of a diverse set of data tools as SQL (SQL Server), Python (Pandas), Figma and Tableau.

It will be done following the Google Analysis Phases Framework, which is summarised in the diagram below

<img src="https://miro.medium.com/max/1200/1*Z7x7l5YL4WbHkxQckLSXlA.png" width="500">

Project tools / libraries:
- Microsoft SQL Server
- Pandas
- Figma
- Tableau

Techniques applied:
- Data extraction
- Data Transformation
- Data Cleaning
- Dashboard design (With Figma)



## Data description

Dataset has been provided by [Maven Analytics](https://www.mavenanalytics.io/data-playground), it contains 6 files: `454_calendar.csv`, `taxi_zones.csv`, `2017_taxi_trips.csv`, `2018_taxi_trips.csv`, `2019_taxi_trips.csv`, `2020_taxi_trips.csv`. The parameters included are:

### 454_calendar.csv
> - **`Date`**: Unique number for each athlete. (categorical)
> - **`FiscalYear`**: Athlete's name. (categorical)
> - **`FiscalQuarter`**: Male (M) or Female (F). (categorical)
> - **`FiscalMonthNumber`**: Athlete's age. (numerical)
> - **`FiscalMonthOfQuarter`**: Athlete's height. (numerical)
> - **`FiscalWeekOfYear`**: Athlete's weight. (numerical)
> - **`DayOfWeek`**: Team name. (categorical)
> - **`FiscalMonthName`**: National Olympic Committee 3-letter code. (categorical)
> - **`FiscalMonthYear`**: Year and season. (categorical)
> - **`FiscalQuarterYear`**: Year of the event. (numerical)
> - **`DayOfMonthNumber`**: Summer or Winter. (categorical)
> - **`DayName`**: Host city. (categorical)

### taxi_zones.csv
> - **`LocationID`**: National Olympic Commitee 3 letter code. (categorical)
> - **`Borough`**: Country name. (categorical)
> - **`Zone`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`service_zone`**: Real country name if "Region" isn't an exact match. (categorical)

### [year] taxi_trips.csv
> - **`VendorID`**: National Olympic Commitee 3 letter code. (categorical)
> - **`lpep_pickup_datetime`**: Country name. (categorical)
> - **`lpep_dropoff_datetime`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`store_and_fwd_flag`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`RatecodeID`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`PULocationID`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`DOLocationID`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`passenger_count`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`trip_distance`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`fare_amount`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`extra`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`mta_tax`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`tip_amount`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`tolls_amount`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`improvement_surcharge`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`total_amount`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`payment_type`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`trip_type`**: Real country name if "Region" isn't an exact match. (categorical)
> - **`congestion_surcharge`**: Real country name if "Region" isn't an exact match. (categorical)
