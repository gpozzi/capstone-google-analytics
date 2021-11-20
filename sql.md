```sql
ALTER TABLE taxis.dbo.[2017_taxi_trips]
ADD congestion_surcharge FLOAT DEFAULT 0.0

UPDATE
	taxis.dbo.[2017_taxi_trips]
SET
    congestion_surcharge = 0
WHERE
    congestion_surcharge IS NULL
```
SELECT
	TOP 50 *
FROM
	taxis.dbo.[2017_taxi_trips]


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

SELECT
	TOP 50 *
FROM taxis.dbo.trips
WHERE
	YEAR(lpep_dropoff_datetime) = 2020

UPDATE
	taxis.dbo.trips
SET passenger_count = CASE passenger_count
						WHEN 0 THEN 1
						ELSE passenger_count
						END

UPDATE
taxis.dbo.trips
SET 
	fare_amount = -fare_amount,
	mta_tax = -mta_tax,
	congestion_surcharge = -congestion_surcharge,
	improvement_surcharge = -improvement_surcharge
WHERE
	fare_amount < 0
	AND mta_tax < 0
	AND congestion_surcharge < 0
	AND improvement_surcharge < 0

UPDATE
	taxis.dbo.trips
SET 
	trip_distance = (fare_amount - 2.5) / 2.5
WHERE
	fare_amount > 0
	AND
	trip_distance = 0

UPDATE
	taxis.dbo.trips
SET 
	fare_amount = 2.5 + (trip_distance * 2.5)
WHERE
	fare_amount = 0
	AND
	trip_distance > 0

DECLARE @tempcol AS datetime
UPDATE
	taxis.dbo.trips
SET
	@tempcol = lpep_pickup_datetime,
	lpep_pickup_datetime = lpep_dropoff_datetime,
	lpep_dropoff_datetime = @tempcol
WHERE
	lpep_dropoff_datetime < lpep_pickup_datetime

SELECT
	TOP 50 *
FROM taxis.dbo.trips
WHERE
	lpep_dropoff_datetime < lpep_pickup_datetime

SELECT TOP 50 *
FROM taxis.dbo.trips
WHERE
	fare_amount < 0
	AND mta_tax < 0
	AND congestion_surcharge < 0
	AND improvement_surcharge < 0

UPDATE
taxis.dbo.trips
SET
	store_and_fwd_flag = REPLACE(store_and_fwd_flag, '"', '')

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

SELECT
	TOP 50 *
FROM
	taxis.dbo.trips_final
WHERE
	lpep_pickup_datetime > '2019-06-01'
	AND
	lpep_dropoff_datetime < '2020-09-01'
	

SELECT
	*
FROM taxis.dbo.trips_final
WHERE
	YEAR(lpep_dropoff_datetime) = 2018 AND MONTH(lpep_dropoff_datetime) = 3 AND DAY(lpep_dropoff_datetime) = 26
ORDER BY lpep_dropoff_datetime
