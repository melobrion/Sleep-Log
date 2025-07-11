# Personal Sleep Log and Blood Pressure

## Questions

### 1. How many nights of sleep were recorded?
````sql
SELECT count(*) AS n_nights
FROM SleepLog;
````

### 2. What time was my earliest bedtime? 
````sql
SELECT Date,
	min(Sleep_time) as earliest_sleep_time,
	Waking_Time,
	Sleep_Duration_mins,
	Sleep_quality,
	Garminn_score
FROM SleepLog
WHERE Sleep_Time < '23:59' AND Sleep_Time >= '18:00';
````

### 3. What was my latest bedtime?
````sql
SELECT Date,
	max(Sleep_time) as latest_sleep_time,
	Waking_Time,
	Sleep_Duration_mins,
	Sleep_quality,
	Garminn_score
FROM SleepLog
WHERE Sleep_Time < '08:00';
````

### 4. Finding my waking time distribution.
````sql
SELECT CASE
    WHEN Waking_Time BETWEEN '04:00' AND '05:59' THEN '4am - 5:59am'
    WHEN Waking_Time BETWEEN '06:00' AND '07:59' THEN '6am - 7:59am'
    WHEN Waking_Time BETWEEN '08:00' AND '09:59' THEN '8am - 9:59am'
    WHEN Waking_Time BETWEEN '10:00' AND '11:59' THEN '10am - 11:59am'
    WHEN Waking_Time BETWEEN '12:00' AND '13:59' THEN '12pm - 1:59pm'
    ELSE 'Other' 
  END AS wake_times,
  COUNT(*) AS count
FROM SleepLog
GROUP BY wake_times
ORDER BY count DESC;
````

### 5. Finding out the minimum, maximum and average sleep quality.
````sql
--Average sleep quality
SELECT count(*) as nights,
	min(Sleep_quality),
	max(Sleep_quality),
	avg(Sleep_quality)
FROM SleepLog;
````




### 5. What is the average sleep duration? Converted from minutes to hours

````sql
--Average sleep duration
WITH average_sleep_duration as (
SELECT avg(Sleep_Duration_mins) as avg_sleep_duration
FROM SleepLog
)
SELECT 
	CAST(avg_sleep_duration / 60 AS INTEGER) || "h" ||
	CAST(avg_sleep_duration % 60 AS INTEGER) || "m" AS sleep_duration_hrs
FROM average_sleep_duration;
````



### 2. How consistent was my bedtime?


````sql
SELECT *
FROM SleepLog
WHERE Sleep_Time BETWEEN  '23:00' AND '23:59';

SELECT
    AVG((strftime('%H', Sleep_Time) * 60 + strftime('%M', Sleep_Time))) AS avg_time,
    AVG((strftime('%H', Sleep_Time) * 60 + strftime('%M', Sleep_Time)) *
        (strftime('%H', Sleep_Time) * 60 + strftime('%M', Sleep_Time))) -
    AVG((strftime('%H', Sleep_Time) * 60 + strftime('%M', Sleep_Time))) *
    AVG((strftime('%H', Sleep_Time) * 60 + strftime('%M', Sleep_Time))) AS variance_time
FROM SleepLog;
````

SELECT count(*) as n_nights,
	avg(Quality_of_Sleep) as avg_sleep_score,
	avg(Systolic) as avg_systolic,
	avg(Diastolic) as avg_diastolic,
	avg(Pulse_Pressure) as avg_pulse_pressure
FROM SleepLog;


SELECT Garminn_Sleep_Score,
	count(*) as sleep_count
FROM SleepLog
GROUP BY Garminn_Sleep_Score;




