# Capstone Project: Dashboard design & Analysis of demand patterns for taxi trips in NY [Work in progress]

## Table of contents

- [Summary](#summary)
  * [Results](#results)
- [The case](#the-case)
  * [1. ASK](#1-ask)
  * [2. PREPARE](#2-prepare)
  * [3. PROCESS](#3-process)
  * [4. ANALYZE](#4-analyze)
  * [5. SHARE](#5-share)
  * [6. ACT](#6-act)

## Summary

<img src="https://user-images.githubusercontent.com/52865532/142680713-61a35034-0413-46e4-ac08-d4c38299c54e.jpg" width="500">

In this page, I'll be documenting a comprehensive analysis of a fictitious business case. This was done as a capstone project for my [Google Data Analytics Professional Certificate](https://www.credly.com/earner/earned/share/8c3b6269-5e44-40d0-be37-e3f9c8e8f3da).

It will follow the Google's Six Analysis Phases Framework, which is summarised in the diagram below and explained with more detail in the following link: https://emkautsar.medium.com/data-analysis-phases-7f32b575b431

<img src="https://miro.medium.com/max/1200/1*Z7x7l5YL4WbHkxQckLSXlA.png" width="500">

Project tools / libraries used:
- **`Microsoft SQL Server`**
- **`Python: Pandas`**
- **`Figma`**
- **`Tableau`**
- **`Google Slides`**

Techniques applied:
- `Data extraction`
- `Data Transformation`
- `Data Cleaning`
- `Dashboard design (With Figma)`
- `Presentation design`
 
Deliverables:
- A clear statement of the business task
- A description of all data sources used
- Documentation of any cleaning or manipulation of data
- A summary of the analysis done
- Supporting visualizations and key findings
- Six relevant recommendations based on my analysis

### Results

<img src="https://user-images.githubusercontent.com/52865532/149670884-93bc8ac3-2e5b-4fc4-93ac-a949c1473df0.png" width="500">

- [**`G Slides presentation`**](https://docs.google.com/presentation/d/1CkPvcjGs-CfqlsqgqwN1xjjvlNPCdZvbQtDTI0GeMME/edit#slide=id.g35f391192_00): results summary for the case
- [**`Tableau Dashboard`**](https://public.tableau.com/app/profile/gonzalo3304/viz/MavenTaxiChallenge_16357997930910/Dashboard1?publish=yes): solicited dashboard

## The case

### 1. ASK
[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

#### Mario Maven's email

For this challenge, I’ll be playing the role of a new Data Analyst for the New York City Taxi & Limousine Commission. It's my first week on the job, and I just received the following email from the Lead Dispatcher:

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

> Once the data is cleaned up, I’m hoping you can build me a dashboard to help with weekly planning and logistics. For any given fiscal week, I'd like to be able to use historical data to answer the following questions:

> - What's the average number of trips we can expect this week?
> - What's the average fare per trip we expect to collect?
> - What's the average distance traveled per trip?
> - How do we expect trip volume to change, relative to last week?
> - Which days of the week and times of the day will be busiest?
> - What will likely be the most popular pick-up and drop-off locations?
> I realize this is a lot to ask for, but this type of analysis will have a huge impact on our business!

> Thanks in advance,

> Mario Maven (Lead Dispatcher, NYC Green Taxis)

#### Business context

##### NYC Taxi & Limousine Commission
<img align="center" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Nyctlc_logo.webp/300px-Nyctlc_logo.webp.png" width="150">
The New York City Taxi and Limousine Commission (NYC TLC) is an agency of the New York City government that licenses and regulates the medallion taxis and for-hire vehicle industries, including app-based companies. The TLC's regulatory landscape includes medallion (yellow) taxicabs, green or Boro taxicabs, black cars (including both traditional and app-based services), community-based livery cars, commuter vans, paratransit vehicles (ambulettes), and some luxury limousines.

###### Mission statement
> Develop and improve taxi and livery service in New York City, by establishing an overall public transportation policy governing taxi, coach and car services and wheelchair-accessible vans, and to establish certain rates and standards.

##### Green taxis
In the summer of 2013, New York City (NYC) introduced the first Boro Taxis to complement the Yellow Taxis. The Boro Taxis are a special class of taxis designed to increase the amount of taxi service provided to **low demand** areas of the city. Boro taxis were introduced because too little taxi service was provided in the outer boroughs. They have a restricted service area that prevents them from picking up passengers in Manhattan, requiring them to provide service to other areas of the city. Boro taxis operate alongside Yellow Taxis.

<img align="center" src="https://user-images.githubusercontent.com/52865532/142743814-decbd896-cc48-48e3-8fa3-9511ad6e6091.png" width="250">

#### Main stakeholders
#####  Me
I've been recently hired as a Data Analyst for the New York City Taxi & Limousine Commission. It's my first week on the job. My main job duty will be to conduct any analysis solicited to support Operations department.

##### Mario Maven
Mario is the Lead Dispatcher and the solicitor of the dashboard. Let's take a look at the job description of a typical Lead Dispatcher:

> Taxi dispatchers, also called starters, send cabs off to customers and keep records of all road-service calls. They may stay in touch with the drivers while they are on the road, communicating by phone, computer, or two-way radio. They help drivers with problems and answer their questions. For example, they may tell drivers which routes to take to avoid traffic jams. When drivers are involved in accidents, dispatchers call for assistance and send other taxis to the customers.

Given the job description, Mario's main interest will be almost entirely operational: to maximize efficiency in resource allocation.

#### Green taxi drivers
Green taxi drivers will be indirectly impacted by the result of the present data analysis. They will be interested in maximizing the fare amount and reducing the time spent on the street without a passenger assigned.

#### Business task
Judging by Mario's interest in only street-hailed trips, the weekly planning will have the objective of preassigning spots for cars that will be looking for passengers on the street. This way, Mario will allocate a specific amount of taxis for each day for each zone and taxi drivers will secure their spots.

A well crafted dashboard will greatly improve weekly planning and logistics, which will in turn impact operational efficiency, by reducing the amount of time taxis spend on the street without a passenger assigned. A well planned day will avoid overcrowding of taxis in areas with more demand than supply and viceversa.

This way, the dashboard will positively impact all stakeholders involved while being aligned with the company's mission statement: to improve taxi and livery service in New York City.

Some other questions will be asked as well to look for relevant insights:
- Do fares depend on the time of day? And what about the day of the week?
- Has the payment type changed over the course of the years?
- Is there a time in which tips are more generous?
- What is the demand like for different boroughs at different times?
- Is trip length the same for all hours of the day?
- 

### 2. PREPARE

[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

#### Data source
We'll assume the data will be pulled from the Company's database. We'll use `SQL Server Management Studio` to write our queries.

#### Data description
The dataset has been provided by [Maven Analytics](https://www.mavenanalytics.io/data-playground) ([**`original source`**](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)), it contains 6 files: `454_calendar.csv`, `taxi_zones.csv`, `2017_taxi_trips.csv`, `2018_taxi_trips.csv`, `2019_taxi_trips.csv`, `2020_taxi_trips.csv`, along with a geospatial map in TopoJSON (to be used with PowerBI) and Shapefile (Tableau) formats. It contains over 28 million Green Taxi trips records registered between 2017 and 2020. The parameters included are:

##### 454_calendar.csv
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

##### taxi_zones.csv
> - **`LocationID`**: TLC Taxi Zone code. (categorical)
> - **`Borough`**: TLC Taxi Borough name. (categorical)
> - **`Zone`**: TLC Taxi Zone name. (categorical)
> - **`service_zone`**: Real country name if "Region" isn't an exact match. (categorical)

##### [year] taxi_trips.csv (4 files):
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

#### Data credibility and integrity
- **Reliability** : This dataset may not be entirely reliable since 2020 demand was severely affected by the COVID-19 outbreak which greatly impacted on the number of people travelling to the office in Manhattan. Special consideration should be taken into which trends may reverse and which ones may stay. Another caveat to take into account is that some of the data is entered manually, so it should be properly checked.

- **Originality**: This data is owned and provided by the NYT&LC. Thus it's original and we can rely it wasn't modified from its collection to its actual usage.

- **Comprehensiveness**: The data is comprehensive, since it provides all the parameters needed to answer the questions stated about taxi service usage.

- **Current**: The data is recent (less than a year old at the time of present analysis) and does represent current trends.

- **Cited**: this dataset has been cited and published on [NYC's open data site](https://data.cityofnewyork.us/Transportation/2016-Green-Taxi-Trip-Data/hvrh-b6nb).

After checking previously stated bullet points, we can see that the data shows a high level of integrity and credibility. We should, however, take into account that 2020's data will be highly impacted by COVID-19, in which we expect to see cab rides severely plummeting because of the fear of infection, widespread adoption of home office throughout Manhattan and lockdowns that restricted citizen's mobility. A revision of this analysis when 2021 data is released should be made to confirm the conclusions. A proper data cleaning has to be done to ensure removal of manual input errors

### 3. PROCESS

[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

#### Tools used
Some preprocessing in **`Pandas`** has been done because the DATETIME format of two columns was not consistent across all rows and tables and with it I could enforce its uniformity.

Most data cleaning was made with **`SQL Server Management Studio`**, since we'll assume data is stored in the Company's database.

Data visualizations was done in **`Tableau Public`**.

Dashboard background design was done with **`Figma`**, by using a [**`free template`**](https://freebiesbug.com/figma-freebies/5-dashboards/) and adapting it to this case's requirements.

#### Data processing
In the first place, the data will be extracted as CSV files to apply some preprocessing in Pandas. The steps involved will be documented [**`here`**](https://github.com/gpozzi/capstone-google-analytics/blob/main/python.md)

We'll then load the `taxi_trips_[year]` datasets into a database as 4 different tables. I didn't concatenate them in Pandas to keep the most part of the preprocessing in SQL.

The data cleaning was documented [**`here`**](https://github.com/gpozzi/capstone-google-analytics/blob/main/sql.md)

### 4. ANALYZE

[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

Now that the data has been preprocessed, we'll put the data to good use as solicited in Mario's email. We'll include the additional questions stated in "business task" section.

We loaded the Shapefile formats and took a look at the service zones Manhattan is divided into. We can see Manhattan is divided into 4 zones:
- Yellow Zone
- Boro Zone
- EWR
- Airports.

As we stated in ASK phase, Green Cabs are only able to hail passengers in "Boro Zone".
<img src="https://user-images.githubusercontent.com/52865532/145698251-26e75272-976f-4da3-9303-2a0866b419a8.png" width="700">

The amount of trips done is a little higher during the first half of the year, but, as we stated before, the demand of the service has recently plummeted because of the COVID-19 outbreak. It is expected to stay low (between the one shown on 2020 and 2019) since a lot of people have been starting to work from home and do not intend to return to Manhattan any time soon.

<img src="https://user-images.githubusercontent.com/52865532/148699214-e5bc437c-b47b-4c51-aa34-c928ac76f832.png" width="700">

As for the payment type variable, since the start of the pandemic, cash usage is being replaced in favor of contactless payment types [apparently because of fear of of COVID-19 being transmitted through physical money](https://www.inquirer.com/business/retail/coronavirus-surge-cashless-mobile-payments-20200706.html).

<img src="https://user-images.githubusercontent.com/52865532/147894398-40d593fe-9dfe-4ca1-bd54-eb2df73c77bf.png" width="700">

Cab demand is higher during worktime and peaks towards 6:00 PM. 

<img src="https://user-images.githubusercontent.com/52865532/147894552-92458ed8-e2ca-42c6-8c47-01ed2a415063.png" width="700">

Significantly longer trips take place at 5:00 - 6:00 AM, which is the time people commute to work. This trend does not mirror at 18:00 PM which is the time at which people return home.

Hypothesis worth further exploration:

1- Boro taxis cannot pick up passengers in Manhattan and so the trip back home is done mainly on yellow cabs.

2- When people return home they combine transportation methods and take cabs as last mile solution. We can hypothesize in the morning people take cabs to get to work faster to be able to sleep more and back home they take subway + cabs or bus + cabs because they're not mostly in a hurry and can save money with cheaper means of transportation.

<img src="https://user-images.githubusercontent.com/52865532/147880239-c4547d86-7c0b-497a-85d8-bdcfd34418f5.png" width="700">

Demand is mostly stable across the days of the week, but we can see when it gets closer to the weekend night trips become more common. This may indicate people go clubbing / visiting friends / get a drink at a bar and stay longer in Manhattan those days.

<img src="https://user-images.githubusercontent.com/52865532/148699294-1dbdd450-1368-4ecc-af59-12507bdb65e6.png" width="700">

Trips tend to last between 18 and 27 minutes, with peaks on rush hours (around 5:00 AM and 4:00 PM)

<img src="https://user-images.githubusercontent.com/52865532/147880638-422db705-a3d8-40d3-a63c-5aa87f30b309.png" width="700">

We can infer for the length of the trips that the ones that are significantly more expensive take place at around 4:00 - 6:00 AM.

<img src="https://user-images.githubusercontent.com/52865532/147894488-2fe9016e-b080-4bdf-91d7-157d56925fb4.png" width="700">

The ratio [**`tip_amount`**]/ [**`trip_distance`**] has been calculated to see at which time are passengers more generous with taxi drivers. It may be useful to be able to identify the most profitable hours to allocate the best taxis (which may be the cleanest, with best service) as a reward and incentivize service improvement.

<img src="https://user-images.githubusercontent.com/52865532/147895641-955b4397-ed6d-4221-80a4-74a3b78bf26f.png" width="700">

As for the location of most of the demand, we'll take a snapshot of the amount of trips that take place at 18:00 which is the peak of it.

<img src="https://user-images.githubusercontent.com/52865532/146680553-2c4ea9c5-6654-4c02-b2ae-b4e14f7c435b.png" width="700">

#### Area 1 (Northern Manhattan)
Area 1 is mostly Harlem / northern Manhattan, which is the northern access. We can see an average hourly demand of between 1.800 and 3.100 trips

<img src="https://user-images.githubusercontent.com/52865532/146680647-718596f0-191d-41fe-ac2e-637991515661.png" width="700">

#### Area 2 (Brooklyn)
Area 2 is the southern access to Manhattan, done via Brooklyn and Manhattan Bridges. We can see an average hourly demand of between 1.300 and 1.600 trips

<img src="https://user-images.githubusercontent.com/52865532/146680815-3ed9fe09-3c66-41e1-bf87-ad568a83a9b6.png" width="700">

#### Area 3 (Queens)
Area 3 concentrates most of the demand around the neighborhoods closest to the fast accesses to Manhattan (Grand Central Parkway and Queensboro Bridge). The average hourly demand is between 1.100 and 2.200 trips

<img src="https://user-images.githubusercontent.com/52865532/146681034-a03244b5-8398-4c8f-b064-48fbc3d4816c.png" width="700">

We also clustered neighborhoods into three groups to see which ones concentrate most trips:

<img src="https://user-images.githubusercontent.com/52865532/147894311-47d99340-eb3c-4179-8a0e-9aa06d43401b.png" width="200">

And we plotted all three groups onto this map

<img src="https://user-images.githubusercontent.com/52865532/147894340-1b5bb88e-e1b3-496a-ad0e-9ab23cf87ad3.png" width="700">

### 5. SHARE

[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

In this phase of the project, I'll be sharing a Google Slides presentation that can be seen [**`here`**](https://docs.google.com/presentation/d/1CkPvcjGs-CfqlsqgqwN1xjjvlNPCdZvbQtDTI0GeMME/edit?usp=sharing)

### 6. ACT

[<img src="https://user-images.githubusercontent.com/52865532/150685377-5ed9dee9-561d-4949-9617-345d0f946174.png" width="200">](#table-of-contents)

As a closing to this project, I'll write a response email to Mario Maven with the information solicited and some recomendations based on the analyzed data

> Hey Mario!

> I'm writing this email to let you know I've finished the task you assigned me. It was tough, but I learned from it and enjoyed it a lot.

> I've also added some other insights to the ones you asked me because I find they can be useful to you.

> If you want to, we can have a 15 minutes meeting via Zoom and I can further explain some things I've seen. I've prepared a Slides presentation with the key insights which you'll be able to see [**`here`**](https://docs.google.com/presentation/d/1CkPvcjGs-CfqlsqgqwN1xjjvlNPCdZvbQtDTI0GeMME/edit?usp=sharing)

> [**`Here`**](https://docs.google.com/presentation/d/1CkPvcjGs-CfqlsqgqwN1xjjvlNPCdZvbQtDTI0GeMME/edit?usp=sharing)'s the Tableau Dashboard you asked me to build to help you with your weekly planning. I've designed the background with Figma and I hope you find it visually appealing and useful.

> Here's some things I recommend based on the data I've been analyzing:

> - Use the clusters I've created to keep track of the different neighborhoods. High demand zones are critical since taxis shortages can have a greater impact on the service level and should be avoided at all costs since they'll affect a lot of people. I'd keep a close eye into these zones and monitor them with more frequency. Low demand zones can be monitored less frequently. Also, knowing the areas where there are more trips you'll be able to reward the best (cleanest, most polite, with best drivers, etc) taxis with these zones to incentivize improvement of the service.

> - You can also assign the best taxis to the hours that tend to tip more.

> - It would be great to incentivize people to stay 1-2 more hours in Manhattan to flatten a little demand at peak hours. With a flatter demand curve, you'd avoid having too many cars on the street for a couple of hours and reduce the possibility of service shortages. Maybe we can consider partnering with pubs to offer discounts at peak hours? Or also partner with gyms to make people stay after work? We can be creative about this because I find this critical. Peaks generate inefficient resources allocation

> - Cashless payment started to gain traction. Maybe we can take advantage of the mass adoption of electronic means of payment to aim for 80% cashless by partnering with more PSPs (Payment Service Providers) such as Paypal / Stripe / PayU / etc and charging a small service fee for cash payment? This can improve taxi service since it's faster to charge, it avoids physical contact and reduces admin time.

> - With all this being said, I'd take all these recommendations with a pinch of salt since a lot has changed in 2020. We'll have to add 2021 data and implement a dashboard in real time to check if a lot of these trends stick after COVID. I expect to see a process of decentralization with a lot of workplaces migrating outside the island with a reduction of trips from and to Manhattan.

> If you want a personal meeting with me so I can explain everything with more detail, you can book one **[`here`]**(https://calendly.com/gonzalopozzi/30min)

> I'm looking forward for the next challenge

> See you soon!
