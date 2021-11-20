# Capstone Project: NYC Green Taxis

# Summary / Scope

<img src="https://user-images.githubusercontent.com/52865532/142680713-61a35034-0413-46e4-ac08-d4c38299c54e.jpg" width="500">

In this page, I'll be documenting a comprehensive analysis of a fictitious business case. This was done as a capstone project for my [Google Data Analytics Professional Certificate](https://www.credly.com/earner/earned/share/8c3b6269-5e44-40d0-be37-e3f9c8e8f3da).

It will follow the Google's Six Analysis Phases Framework, which is summarised in the diagram below and explained with more detail in the following link: https://emkautsar.medium.com/data-analysis-phases-7f32b575b431

<img src="https://miro.medium.com/max/1200/1*Z7x7l5YL4WbHkxQckLSXlA.png" width="500">

Project tools / libraries used:
- `Microsoft SQL Server`
- `Python: Pandas`
- `Figma`
- `Tableau`

Techniques applied:
- `Data extraction`
- `Data Transformation`
- `Data Cleaning`
- `Dashboard design (With Figma)`

# Data description
The dataset has been provided by [Maven Analytics](https://www.mavenanalytics.io/data-playground), it contains 6 files: `454_calendar.csv`, `taxi_zones.csv`, `2017_taxi_trips.csv`, `2018_taxi_trips.csv`, `2019_taxi_trips.csv`, `2020_taxi_trips.csv`, along with a geospatial map in TopoJSON (to be used with PowerBI) and Shapefile (Tableau) formats. It contains over 28 million Green Taxi trips records registered between 2017 and 2020. The parameters included are:

## 454_calendar.csv
> - **`Date`**: Date [`2018-02-03`]. (categorical)
> - **`FiscalYear`**: Year number[`2018`]. (categorical)
> - **`FiscalQuarter`**: Quarter number of current fiscal year [`02`]. (categorical)
> - **`FiscalMonthNumber`**: Month number of current fiscal year [`10`]. (numerical)
> - **`FiscalMonthOfQuarter`**: Month number of current fiscal quarter [`2`]. (numerical)
> - **`FiscalWeekOfYear`**: Week number of current fiscal year [`20`]. (numerical)
> - **`DayOfWeek`**: Number of day [`7`]. (numerical)
> - **`FiscalMonthName`**: Month name of current fiscal year [`June`]. (categorical)
> - **`FiscalMonthYear`**: Fiscal year and month name [`18-Jun`]. (categorical)
> - **`FiscalQuarterYear`**: Quarter number and year [`22018`]. (numerical)
> - **`DayOfMonthNumber`**: Number of day in current month [`10`]. (categorical)
> - **`DayName`**: Name of day [`Sunday`]. (categorical)

## taxi_zones.csv
> - **`LocationID`**: TLC Taxi Zone code. (categorical)
> - **`Borough`**: TLC Taxi Borough name. (categorical)
> - **`Zone`**: TLC Taxi Zone name. (categorical)
> - **`service_zone`**: Real country name if "Region" isn't an exact match. (categorical)

## [year] taxi_trips.csv (4 files):
> - **`VendorID`**: A code indicating the LPEP provider that provided the record (1= Creative Mobile Technologies, LLC,  2= Verifone Inc.)". (categorical)
> - **`lpep_pickup_datetime`**: The date and time when the meter was engaged. (categorical)
> - **`lpep_dropoff_datetime`**: The date and time when the meter was disengaged. (categorical)
> - **`store_and_fwd_flag`**: This flag indicates whether the trip record was held in vehicle memory before sending to the vendor, aka “store and forward,” because the vehicle did not have a connection to the server (Y= store and forward trip, N= not a store and forward trip)". (categorical)
> - **`RatecodeID`**: The final rate code in effect at the end of the trip (1= Standard rate, 2= JFK, 3= Newark, 4= Nassau or Westchester, 5= Negotiated fare, 6= Group ride). (categorical)
> - **`PULocationID`**: TLC Taxi Zone in which the taximeter was engaged. (categorical)
> - **`DOLocationID`**: TLC Taxi Zone in which the taximeter was disengaged. (categorical)
> - **`passenger_count`**: The number of passengers in the vehicle (this is a driver entered value). (categorical)
> - **`trip_distance`**: The elapsed trip distance in miles reported by the taximeter. (categorical)
> - **`fare_amount`**: The time-and-distance fare calculated by the meter. (categorical)
> - **`extra`**: Miscellaneous extras and surcharges (this only includes the $0.50 and $1 rush hour and overnight charges). (categorical)
> - **`mta_tax`**: $0.50 MTA tax that is automatically triggered based on the metered rate in use. (categorical)
> - **`tip_amount`**: Tip amount (automatically populated for credit card tips - cash tips are not included). (categorical)
> - **`tolls_amount`**: Total amount of all tolls paid in trip. (categorical)
> - **`improvement_surcharge`**: $0.30 improvement surcharge assessed on hailed trips at the flag drop. (categorical)
> - **`total_amount`**: The total amount charged to passengers (does not include cash tips). (categorical)
> - **`payment_type`**: A numeric code signifying how the passenger paid for the trip (1= Credit card, 2= Cash, 3= No charge, 4= Dispute, 5= Unknown, 6= Voided trip). (categorical)
> - **`trip_type`**: A code indicating whether the trip was a street-hail or a dispatch that is automatically assigned based on the metered rate in use but can be altered by the driver (1= Street-hail, 2= Dispatch). (categorical)
> - **`congestion_surcharge`**: Congestion surcharge for trips that start, end or pass through the congestion zone in Manhattan, south of 96th street ($2.50 for non-shared trips in Yellow Taxis, $2.75 for non-shared trips in Green Taxis)". (categorical)

# Analysis

## ASK Phase
> For the Maven Taxi Challenge, you’ll be playing the role of a new Data Analyst for the New York City Taxi & Limousine Commission. It's your first week on the job, and you just received the following email from the Lead Dispatcher
> We’ve been collecting trip data for ~4 years now, but without a proper analyst we haven’t been able to put it to good use. That's where you come in!
> I’m hoping you can build me a dashboard to help with weekly planning and logistics

### Protagonists:
####  Me
I've been recently hired as a Data Analyst for the New York City Taxi & Limousine Commission. It's my first week on the job.

#### NYC Taxi & Limousine Commission


#### Mario Maven
Mario is the Lead Dispatcher and the solicitor of the dashboard. Let's take a look at the 

- Dispatcher role: Taxi dispatchers, also called starters, send cabs off to customers and keep records of all road-service calls. They may stay in touch with the drivers while they are on the road, communicating by phone, computer, or two-way radio. They help drivers with problems and answer their questions. For example, they may tell drivers which routes to take to avoid traffic jams. When drivers are involved in accidents, dispatchers call for assistance and send other taxis to the customers.

Read more: Taxi Dispatcher Job Description, Career as a Taxi Dispatcher, Salary, Employment - Definition and Nature of the Work, Education and Training Requirements, Getting the Job - StateUniversity.com https://careers.stateuniversity.com/pages/790/Taxi-Dispatcher.html#ixzz7CltXiIUu

Business goal: achieve operational efficiency by efficiently allocating taxis to different zones so they will have to move less to pick up a passenger.
Pain point: 

Problem statement:
A well crafted dashboard will greatly improve weekly planning and logistics, which is what the Lead Dispatcher will

## PREPARE Phase

## PROCESS Phase

## ANALYZE Phase

## SHARE Phase

## ACT Phase
