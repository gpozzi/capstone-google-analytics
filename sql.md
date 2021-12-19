It seems that the variable `congestion_surcharge` was added in 2018. We need to artificially add it to the 2017 dataset to be able to concatenate all tables into one.

```sql
ALTER TABLE taxis.dbo.[2017_taxi_trips]
ADD congestion_surcharge FLOAT
```

We'll set the default value of `congestion_surcharge` as 0.0
```sql
UPDATE
	taxis.dbo.[2017_taxi_trips]
SET
    congestion_surcharge = 0.0
WHERE
    congestion_surcharge IS NULL
```

We'll then concatenate all four tables into one, which we'll call **`trips`**
```sql
SELECT
	*
INTO
	taxis.dbo.trips
FROM(
	SELECT
		*
	FROM
		taxis.dbo.[2020_taxi_trips]
	UNION ALL
	SELECT
		*
	FROM
		taxis.dbo.[2019_taxi_trips]
    UNION ALL
	SELECT
		*
	FROM
		taxis.dbo.[2017_taxi_trips]
    UNION ALL
	SELECT
		*
	FROM
		taxis.dbo.[2018_taxi_trips]
    ) AS S;
```

We'll assume that a passenger count of 0 is a typo, and set it as 1 since it's the minimum amount of persons that can start a trip.
```sql
UPDATE
	taxis.dbo.trips
SET passenger_count = CASE passenger_count
						WHEN 0 THEN 1
						ELSE passenger_count
						END
```

When the fare, taxes and congestion surcharge are all negatives, we'll make them positive
```sql
UPDATE
taxis.dbo.trips
SET 
	fare_amount = -fare_amount,
	mta_tax = -mta_tax,
	congestion_surcharge = -congestion_surcharge
WHERE
	fare_amount < 0
	AND mta_tax < 0
	AND congestion_surcharge < 0
```

When there's a trip distance of 0 but a fare which is more than 0, we'll assume a trip has taken place where the distance has not been properly recorded. We'll calculate it this way: **`trip_distance = (Fare_amt - 2.5) / 2.5`**
```sql
UPDATE
	taxis.dbo.trips
SET 
	trip_distance = (fare_amount - 2.5) / 2.5
WHERE
	fare_amount > 0
	AND
	trip_distance = 0
```

When there's a fare of 0 but a distance which is more than 0, we'll assume a trip has taken place where the fare has not been properly recorded. We'll calculate it this way: **`fare_amt = 2.5 + (trip_distance x 2.5)`**
```sql
UPDATE
	taxis.dbo.trips
SET 
	fare_amount = 2.5 + (trip_distance * 2.5)
WHERE
	fare_amount = 0
	AND
	trip_distance > 0
```

When the pickup datetime comes after the dropoff datetime, we'll assume it hasn't been properly recorded and reverse it.
```sql
DECLARE @tempcol AS datetime
UPDATE
	taxis.dbo.trips
SET
	@tempcol = lpep_pickup_datetime,
	lpep_pickup_datetime = lpep_dropoff_datetime,
	lpep_dropoff_datetime = @tempcol
WHERE
	lpep_dropoff_datetime < lpep_pickup_datetime
```

Since store_and_fwd_flag attribute comes as a string sorrounded by double quotes, we'll remove them to clean it.
```sql
UPDATE
taxis.dbo.trips
SET
	store_and_fwd_flag = REPLACE(store_and_fwd_flag, '"', '')
```

Finally, we'll select the data the Lead Dispatcher has selected as valuable:
- Trips NOT sent via "store and forward"
- Street-hailed trips paid by card or cash, with a standard rate
- Trips between 2017 and 2020 to and from not-unknown zones
- Trips shorter than a day (less than 86400 seconds) with a distance and fare amount of more than 0
```sql
SELECT
	*
INTO taxis.dbo.trips_final
FROM(
	SELECT
		*
	FROM
		taxis.dbo.trips
	WHERE

	--- Let’s stick to trips that were NOT sent via “store and forward”
		store_and_fwd_flag = 'N'

	--- I’m only interested in street-hailed trips paid by card or cash, with a standard rate
		AND
		trip_type = 1
		AND
		(payment_type = 1 OR payment_type = 2)
		AND
		RatecodeID = 1
		AND

	--- We can remove any trips with dates before 2017 or after 2020, along with any trips with pickups or drop-offs into unknown zones
		lpep_pickup_datetime > '2017-01-01'
		AND
		lpep_dropoff_datetime < '2020-12-31'
		AND
		PULocationID NOT IN ('265','264')
		AND
		DOLocationID NOT IN ('265','264')

	--- We can remove trips lasting longer than a day, and any trips which show both a distance and fare amount of 0
		AND
		DATEDIFF(SECOND,lpep_pickup_datetime,lpep_dropoff_datetime) < 86400
		AND
		(trip_distance != 0 AND fare_amount != 0)) AS S;
```

Having the relevant data selected and cleaned, we'll save the results of the previous query in a .csv file and load it into Tableau to design the dashboard.
