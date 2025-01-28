# de-zoomcamp
Solutions for data engineering zoomcamp cohort 2025

Q3

```sql
-- up to 1 mile trips

select 
count(1)

from public.green_taxi_trips 

where lpep_dropoff_datetime::date between '2019-10-01' and '2019-10-31' 
  and trip_distance  <= 1


-- In between 1 (exclusive) and 3 miles (inclusive)

select 
count(1)

from public.green_taxi_trips 

where lpep_dropoff_datetime::date between '2019-10-01' and '2019-10-31' 
  and trip_distance > 1 and trip_distance <= 3


-- In between 3 (exclusive) and 7 miles (inclusive)

select 
count(1)

from public.green_taxi_trips 

where lpep_dropoff_datetime::date between '2019-10-01' and '2019-10-31' 
  and trip_distance > 3 and trip_distance <= 7

-- In between 7 (exclusive) and 10 miles (inclusive)

select 
count(1)

from public.green_taxi_trips 

where lpep_dropoff_datetime::date between '2019-10-01' and '2019-10-31' 
  and trip_distance > 7 and trip_distance <= 10


--Over 10 miles
select 
--lpep_dropoff_datetime::date,
count(1)

from public.green_taxi_trips 

where lpep_dropoff_datetime::date between '2019-10-01' and '2019-10-31' 
and trip_distance > 10
--group by lpep_dropoff_datetime::date
;
```



Q4
```sql
select lpep_pickup_datetime::date 
from public.green_taxi_trips
where trip_distance = (select max(trip_distance) from public.green_taxi_trips);
```

Q5
```sql
with top_loc as (

select "Borough" || '/' || "Zone" as pickup_loc, sum(total_amount) total_amount
   from public.green_taxi_trips t
   join public.zones z on t."PULocationID" = z."LocationID"
   where "total_amount" > 13000
   group by 1   
   order by "total_amount" desc
)

select lpep_pickup_datetime::date, "Borough" || '/' || "Zone" as pickup_loc, sum(total_amount) total_amount
from public.green_taxi_trips t
   join public.zones z on t."PULocationID" = z."LocationID"

where lpep_pickup_datetime::date = '2019-10-18' --and "Borough" || '/' || "Zone" in (select * from top_loc) 
group by lpep_pickup_datetime::date,"Borough", "Zone"
order by 2 desc
limit 3
```


Q6

```sql
select zz."Zone" as drop_loc, sum(tip_amount) tip
from public.green_taxi_trips t
   join public.zones z on t."PULocationID" = z."LocationID"
   join public.zones zz on t."DOLocationID" = zz."LocationID"

   where z."Zone" = 'East Harlem North'
   group by 1
   order by "tip" desc
select lpep_pickup_datetime::date, "Borough" || '/' || "Zone" as pickup_loc, sum(total_amount) total_amount
from public.green_taxi_trips t
   join public.zones z on t."PULocationID" = z."LocationID"

where lpep_pickup_datetime::date = '2019-10-18' --and "Borough" || '/' || "Zone" in (select * from top_loc) 
group by lpep_pickup_datetime::date,"Borough", "Zone"
order by 2 desc
limit 3
```
