```python
import datetime
import pandas as pd
```

```python
direc = '...\\2017_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['congestion_surcharge'] = 0
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2020_modif.csv',mode='w',index=False)
```

```python
direc = '...\\2018_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['congestion_surcharge'] = 0
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2020_modif.csv',mode='w',index=False)
```

```python
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2020_modif.csv',mode='w',index=False)
```
