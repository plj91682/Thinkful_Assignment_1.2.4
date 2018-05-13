# Thinkful_Assignment_1.2.4
Joins and CTEs

## 1.  What are the three longest trips on rainy days?

```sql
with 
total_precip as (
select date, sum(precipitationin) as total_rain from weather 
group by date )
,
sorted_trips as (
select trip_id, strftime('%Y-%m-%d',start_date) as date, duration from trips
order by duration desc
)
select * from total_precip tp 
join sorted_trips ts on ts.date = tp.date
where total_rain > 0
```

result:

|                                                                                                                 |        |         |           |       |                                            |                                   |         | 
|-----------------------------------------------------------------------------------------------------------------|--------|---------|-----------|-------|--------------------------------------------|-----------------------------------|---------| 
| "date"  |      "total rain"  | "duration" | "trip_id"  | "bike id" |  "start station" |  "end station" |  "zip code"  |        |         |           |       |                                            |                                   |         | 
| "2016-04-22"                                                                                                    | "0.77" | "85900" | "1173890" | "398" | "San Francisco Caltrain (Townsend at 4th)" | "Grant Avenue at Columbus Avenue" | "94133" | 
| "2015-11-15"                                                                                                    | "1.29" | "84349" | "1009870" | "158" | "South Van Ness at Market"                 | "South Van Ness at Market"        | "94122" | 
| "2016-05-21"                                                                                                    | "0.14" | "83915" | "1210487" | "68"  | "Palo Alto Caltrain Station"               | "Palo Alto Caltrain Station"      | "94301" | 

## 2. Which station is full most often?

```sql
with dock_full as (
select count(station_id) as full_count, station_id from status
where docks_available = 0
group by station_id
)
select * from dock_full order by full_count desc
limit 1
```

result:

| "full count" | "station id" | 
|--------------|--------------| 
| "23450"      | "70"         | 

## 3. Return a list of stations with a count of number of trips starting at that station but ordered by dock count

```sql
with 
dockcount_station as (
select dockcount, station_id, name from stations 
order by dockcount asc
)
select ds.* from trips t
join dockcount_station ds on ds.station_id = t.start_terminal
limit 10
```

result:

| "dockcount" | "station id" | "name"                             | 
|-------------|--------------|------------------------------------| 
| "11"        | "37"         | "Cowper at University"             | 
| "11"        | "32"         | "Castro Street and El Camino Real" | 
| "11"        | "35"         | "University and Emerson"           | 
| "11"        | "37"         | "Cowper at University"             | 
| "11"        | "4"          | "Santa Clara at Almaden"           | 
| "11"        | "32"         | "Castro Street and El Camino Real" | 
| "11"        | "4"          | "Santa Clara at Almaden"           | 
| "11"        | "37"         | "Cowper at University"             | 
| "11"        | "37"         | "Cowper at University"             | 
| "11"        | "4"          | "Santa Clara at Almaden"           | 

## 4. (Challenge) What's the length of the longest trip for each day it rains anywhere?

```sql
with 
total_precip as (
select date, sum(precipitationin) as total_rain from weather 
group by date 
),
sorted_trips as (
select duration, trip_id, bike_id, start_station, end_station, zip_code, strftime('%Y-%m-%d',start_date) as date from trips
order by duration desc
)
select max(t.duration) , t.date from total_precip tp 
join sorted_trips t on t.date = tp.date
where total_rain > 0
group by t.date
order by t.date
limit 10
```

result:

| "max(t.duration)" | "date"       | 
|-------------------|--------------| 
| "25667"           | "2015-09-30" | 
| "51081"           | "2015-10-01" | 
| "11107"           | "2015-10-27" | 
| "22801"           | "2015-10-28" | 
| "43899"           | "2015-11-01" | 
| "12246"           | "2015-11-02" | 
| "7874"            | "2015-11-08" | 
| "12838"           | "2015-11-09" | 
| "61234"           | "2015-11-10" | 
| "84349"           | "2015-11-15" | 
