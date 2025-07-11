# Personal Sleep Log and Blood Pressure

## Questions

### 1. How many nights of sleep were recorded?
````sql
SELECT count(*) AS n_nights
FROM SleepLog;
````
<img width="103" height="68" alt="DB_Browser_for_SQLite_oiw94p0L7C" src="https://github.com/user-attachments/assets/4ff1e8ae-02e1-4e24-aa55-56b330f41ca9" />

### 2. What time was my earliest bedtime? 
````sql
SELECT Date,
	min(Sleep_time) as earliest_sleep_time,
	Waking_Time,
	CAST(Sleep_Duration_mins / 60 AS INTEGER) || "h " ||
	(Sleep_Duration_mins % 60) || "m" AS sleep_duration_hrs,
	Sleep_score,
	Garminn_sleep_quality
FROM SleepLog
WHERE Sleep_Time < '23:59' AND Sleep_Time >= '18:00';
````
<img width="788" height="65" alt="DB_Browser_for_SQLite_jP00itE3Bj" src="https://github.com/user-attachments/assets/8d4f6fc4-58d3-472f-96c3-a619392177f8" />

### 3. What was my latest bedtime?
````sql
SELECT Date,
	max(Sleep_time) as latest_sleep_time,
	Waking_Time,
	CAST(Sleep_Duration_mins / 60 AS INTEGER) || "h " || 
	(Sleep_Duration_mins % 60) || "m" AS sleep_duration_hrs,
	Sleep_score,
	Garminn_sleep_quality
FROM SleepLog
WHERE Sleep_Time < '08:00';
````
<img width="778" height="63" alt="DB_Browser_for_SQLite_B9EM6W7fQs" src="https://github.com/user-attachments/assets/8e25c680-f40a-458e-91d4-de3232a8b8ea" />


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
<img width="223" height="192" alt="DB_Browser_for_SQLite_7t7ZgoNET4" src="https://github.com/user-attachments/assets/4f54983b-b311-4154-b290-97c421192815" />

### 5. Finding out the minimum, maximum and average sleep quality.
````sql
SELECT count(*) as nights,
	min(Sleep_score) as minimum_sleep_score,
	max(Sleep_score) as maximum_sleep_score,
	avg(Sleep_score) as average_sleep_score
FROM SleepLog;
````

<img width="565" height="66" alt="DB_Browser_for_SQLite_3y2mVPkhfU" src="https://github.com/user-attachments/assets/ea9169db-9ead-434d-9ffe-97086143f420" />



### 6. What is the average sleep duration? Converted from minutes to hours

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
<img width="166" height="59" alt="DB_Browser_for_SQLite_EmNPtmCGJ9" src="https://github.com/user-attachments/assets/d20264ba-3ec2-45d7-8bbf-959c99174613" />



### 7. Blood pressure and Pulse pressure averages.


````sql
-- average blood pressure
SELECT count(*) as n_nights,
	avg(Systolic) as avg_systolic,
	avg(Diastolic) as avg_diastolic,
	avg(Pulse_Pressure) as avg_pulse_pressure
FROM SleepLog;
````
<img width="445" height="62" alt="DB_Browser_for_SQLite_KUSidLQQRs" src="https://github.com/user-attachments/assets/1ccbb6d2-2271-4301-aa1d-0591fec5835a" />







