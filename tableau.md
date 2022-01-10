## Design considerations
- Since the dispatcher will use the dashboard for weekly planning, all data will be filtered by week, considering the average of the four previous years. 
- The color scheme will be according to the same apple green the taxis are colored in.

## Calculated fields

count_trips_borough
```
{FIXED [Borough2],[Lpep Pickup Datetime]:
COUNT(
    IF 
        DATEPART('year',[Lpep Pickup Datetime]) < DATEPART('year',TODAY())
    AND
        DATEPART('year',[Lpep Pickup Datetime]) >= 2017
    AND
        DATEPART('year',[Lpep Pickup Datetime]) < 2021
    AND
        (DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY())
        OR
        DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY()-7))
    THEN
        [Lpep Pickup Datetime]
    ELSE
        NULL
    END
)}
```

----------- 

dist_tips

```
[Tip Amount]/[Trip Distance]
```

<img src="https://user-images.githubusercontent.com/52865532/147895641-955b4397-ed6d-4221-80a4-74a3b78bf26f.png" width="500">

----------- 

percent_creditcard

```
COUNT(IF [Payment Type] == 1
THEN [Lpep Pickup Datetime]
ELSE NULL
END)
/
COUNT([Lpep Pickup Datetime])
```

<img src="https://user-images.githubusercontent.com/52865532/147894398-40d593fe-9dfe-4ca1-bd54-eb2df73c77bf.png" width="500">

----------- 

trips_count_hour

```
{FIXED DATEPART('hour',[Lpep Pickup Datetime]):
COUNT(
    IF 
        DATEPART('year',[Lpep Pickup Datetime]) < DATEPART('year',TODAY())
    AND
        DATEPART('year',[Lpep Pickup Datetime]) >= 2017
    AND
        DATEPART('year',[Lpep Pickup Datetime]) < 2021
    AND
        (DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY())
        OR
        DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY()-7))
    THEN
        [Lpep Pickup Datetime]
    ELSE
        NULL
    END
)}

<img src="https://user-images.githubusercontent.com/52865532/147894552-92458ed8-e2ca-42c6-8c47-01ed2a415063.png" width="500">

```

----------- 

trips_count_this_week (+ clustering into 3 groups)

```
{FIXED DATEPART('hour',[Lpep Pickup Datetime]),DATEPART('week',[Lpep Pickup Datetime]),[Zone]:
COUNT(
    IF 
        DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY())
    THEN
        [Lpep Pickup Datetime]
    ELSE
        NULL
    END
)}/4

```


<img src="https://user-images.githubusercontent.com/52865532/148700249-ff3b53da-08fb-4154-b561-c95d25596a9d.png" width="500">

----------- 

trips_distance_hour
```
{FIXED DATEPART('hour',[Lpep Pickup Datetime]):
AVG(
    IF 
        DATEPART('week',[Lpep Pickup Datetime]) = DATEPART('week',TODAY())
    THEN
        [Trip Distance]
    ELSE
        NULL
    END
)}
```

<img src="https://user-images.githubusercontent.com/52865532/147880239-c4547d86-7c0b-497a-85d8-bdcfd34418f5.png" width="500">

----------- 

trips_duration_hour

```

```


----------- 
