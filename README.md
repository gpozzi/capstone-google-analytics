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

# Analysis

## Mario Maven's email
> Welcome to the team!

> We’ve been collecting trip data for ~4 years now, but without a proper analyst we haven’t been able to put it to good use. That's where you come in!

> The raw data has some issues, so we'll need to make the following adjustments and assumptions to clean and prep the data:

> - Let’s stick to trips that were NOT sent via “store and forward”
> - I’m only interested in street-hailed trips paid by card or cash, with a standard rate
> - We can remove any trips with dates before 2017 or after 2020, along with any trips with pickups or drop-offs into unknown zones
> - Let’s assume any trips with no recorded passengers had 1 passenger
> - If a pickup date/time is AFTER the drop-off date/time, let’s swap them
> - We can remove trips lasting longer than a day, and any trips which show both a distance and fare amount of 0
> - If you notice any records where the fare, taxes, and surcharges are ALL negative, please make them positive
> - For any trips that have a fare amount but have a trip distance of 0, calculate the distance this way: (Fare amount - 2.5) / 2.5
> - For any trips that have a trip distance but have a fare amount of 0, calculate the fare amount this way: 2.5 + (trip distance x 2.5)
> - Once the data is cleaned up, I’m hoping you can build me a dashboard to help with weekly planning and logistics. For any given fiscal week, I'd like to be able to use historical data to answer the following questions:

> - What's the average number of trips we can expect this week?
> - What's the average fare per trip we expect to collect?
> - What's the average distance traveled per trip?
> - How do we expect trip volume to change, relative to last week?
> - Which days of the week and times of the day will be busiest?
> - What will likely be the most popular pick-up and drop-off locations?
> I realize this is a lot to ask for, but this type of analysis will have a huge impact on our business!

> Thanks in advance,

> Mario Maven (Lead Dispatcher, NYC Green Taxis)



## 1. ASK Phase

### Business context
#### NYC Taxi & Limousine Commission
<img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Nyctlc_logo.webp/300px-Nyctlc_logo.webp.png" width="150">
The New York City Taxi and Limousine Commission (NYC TLC) is an agency of the New York City government that licenses and regulates the medallion taxis and for-hire vehicle industries, including app-based companies. The TLC's regulatory landscape includes medallion (yellow) taxicabs, green or Boro taxicabs, black cars (including both traditional and app-based services), community-based livery cars, commuter vans, paratransit vehicles (ambulettes), and some luxury limousines.

##### Mission statement
> Develop and improve taxi and livery service in New York City, by establishing an overall public transportation policy governing taxi, coach and car services and wheelchair-accessible vans, and to establish certain rates and standards.

#### Green taxis
In the summer of 2013, New York City (NYC) introduced the first Boro Taxis to complement the Yellow Taxis. The Boro Taxis are a special class of taxis designed to increase the amount of taxi service provided to **low demand** areas of the city. Boro taxis were introduced because too little taxi service was provided in the outer boroughs. They have a restricted service area that prevents them from picking up passengers in Manhattan, requiring them to provide service to other areas of the city. Boro taxis operate alongside Yellow Taxis.

<img align="center" src="https://user-images.githubusercontent.com/52865532/142743814-decbd896-cc48-48e3-8fa3-9511ad6e6091.png" width="250">

### Main stakeholders
####  Me
I've been recently hired as a Data Analyst for the New York City Taxi & Limousine Commission. It's my first week on the job. My main job duty will be to conduct any analysis solicited to support Operations department.

#### Mario Maven
Mario is the Lead Dispatcher and the solicitor of the dashboard. Let's take a look at the job description of a typical Lead Dispatcher:

> Taxi dispatchers, also called starters, send cabs off to customers and keep records of all road-service calls. They may stay in touch with the drivers while they are on the road, communicating by phone, computer, or two-way radio. They help drivers with problems and answer their questions. For example, they may tell drivers which routes to take to avoid traffic jams. When drivers are involved in accidents, dispatchers call for assistance and send other taxis to the customers.

Given the job description, Mario's main interest will be almost entirely operational: to maximize efficiency in resource allocation.

### Green taxi drivers
Green taxi drivers will be indirectly impacted by the result of the present data analysis. They will be interested in maximizing the fare amount and reducing the time spent on the street without a passenger assigned.

### Solution - Success criteria
Judging by Mario's interest in only street-hailed trips, the weekly planning will have the objective of preassigning spots for cars that will be looking for passengers on the street. This way, Mario will allocate a specific amount of taxis for each day for each zone and taxi drivers will secure their spots.

A well crafted dashboard will greatly improve weekly planning and logistics, which will in turn impact operational efficiency, by reducing the amount of time taxis spend on the street without a passenger assigned. A well planned day will avoid overcrowding of taxis in areas with more demand than supply and viceversa.

This way, the dashboard will positively impact all stakeholders involved while being aligned with the company's mission statement: to improve taxi and livery service in New York City.

## 2. PREPARE Phase

### Data source
We'll assume the data will be pulled from the Company's database. We'll use `SQL Server Management Studio` to write our queries.

### Data description
The dataset has been provided by [Maven Analytics](https://www.mavenanalytics.io/data-playground) ([**`original source`**](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)), it contains 6 files: `454_calendar.csv`, `taxi_zones.csv`, `2017_taxi_trips.csv`, `2018_taxi_trips.csv`, `2019_taxi_trips.csv`, `2020_taxi_trips.csv`, along with a geospatial map in TopoJSON (to be used with PowerBI) and Shapefile (Tableau) formats. It contains over 28 million Green Taxi trips records registered between 2017 and 2020. The parameters included are:

#### 454_calendar.csv
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

#### taxi_zones.csv
> - **`LocationID`**: TLC Taxi Zone code. (categorical)
> - **`Borough`**: TLC Taxi Borough name. (categorical)
> - **`Zone`**: TLC Taxi Zone name. (categorical)
> - **`service_zone`**: Real country name if "Region" isn't an exact match. (categorical)

#### [year] taxi_trips.csv (4 files):
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

## 3. PROCESS Phase
### Tools used
A little preprocessing was done in **`Pandas`** because the DATETIME format of two columns was not consistent across all rows and tables and with this tool I could enforce its uniformity.

Most data cleaning was done with **`SQL Server Management Studio`**, since we'll assume data is stored in the Company's database.

### Data processing
In the first place, the data will be extracted as CSV files to apply some preprocessing in Pandas. The steps involved will be documented [**`here`**](https://github.com/gpozzi/capstone-google-analytics/blob/main/python.md)

We'll then load the trips datasets into a database as 4 different tables. I didn't concatenate them in Pandas to keep the most part of the preprocessing in SQL.

The data cleaning was documented [**`here`**](https://github.com/gpozzi/capstone-google-analytics/blob/main/sql.md)

## 4. ANALYZE Phase

## 5. SHARE Phase

## 6. ACT Phase
