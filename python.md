```python
df['congestion_surcharge'] = 0
df['lpep_pickup_datetime'] = df['lpep_pickup_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df['lpep_dropoff_datetime'] = df['lpep_dropoff_datetime'].apply(lambda x:datetime.datetime.strptime(x,'%Y-%m-%d %H:%M:%S.%f'))
df.to_csv('2020_modif.csv',mode='w',index=False)
```
