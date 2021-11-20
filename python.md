We'll first import the libraries we'll use
```python
import datetime
import pandas as pd
```

For the first two CSV files (**`2017_taxi_trips.csv`** and **`2018_taxi_trips.csv`**), we'll first add the attribute `congestion_surcharge`, which isn't present in both tables since it started to be used in 2019. Its value for both years will be 0.

We'll also parse both datetime columns (**`lpep_pickup_datetime`** and **`lpep_dropoff_datetime`**) with the given format `'%Y-%m-%d %H:%M:%S.%f'` to ensure data consistency across all rows.

Finally, they will all be saved as new CSV files.

### 2017
```python
direc = '...\\2017_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['congestion_surcharge'] = 0
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2017_modif.csv',mode='w',index=False)
```

### 2018
```python
direc = '...\\2018_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['congestion_surcharge'] = 0
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2018_modif.csv',mode='w',index=False)
```

### 2019
```python
direc = '...\\2019_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2019_modif.csv',mode='w',index=False)
```

### 2020
```python
direc = '...\\2019_taxi_trips.csv'
df = pd.read_csv(direc,index_col=0)
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2020_modif.csv',mode='w',index=False)
```
