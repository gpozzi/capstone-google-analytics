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