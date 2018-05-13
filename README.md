# Thinkful_Assignment_1.2.4
Joins and CTEs

## 1.  What are the three longest trips on rainy days?

```
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

```
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
