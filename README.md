# Personal Sleep Log and Blood Pressure



<img width="1920" height="1080" alt="Sleep time" src="https://github.com/user-attachments/assets/0785f9ee-7aa7-4d9f-b8c8-03377116f719" />



<img width="1920" height="1080" alt="Duration vs Blood Pressure" src="https://github.com/user-attachments/assets/68708134-bdff-46a1-a72f-92cd33c3c311" />

<img width="12" height="561" alt="PBIDesktop_02WzU33inc" src="https://github.com/user-attachments/assets/5bfd84fd-7496-49e5-8cd1-febab050f64a" />



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





### 5. Sleep time distribution with sleep score and blood pressure averages.

````sql
-- Sleep window distribution
SELECT 
	CASE 
		WHEN Sleep_Time BETWEEN '18:00' AND '21:59' THEN "before 10pm"
		WHEN Sleep_Time BETWEEN '22:00' AND '22:59'  THEN "10pm to 10:59pm"
		WHEN Sleep_Time BETWEEN '23:00' AND '23:59'  THEN "11pm to 11:59pm"
		WHEN Sleep_Time BETWEEN '00:00' AND '00:59'  THEN "12am to 12:59am"
		WHEN Sleep_Time BETWEEN '01:00' AND '01:59'  THEN "1am to 1:59am"
		WHEN Sleep_Time >= '02:00' THEN "after 2am"
	ELSE "Other" 
	END AS sleep_window,
	count(*) AS count,
	round(avg(Sleep_Duration_mins)) AS avg_sleep_duration,
	round(avg(Sleep_score)) AS avg_sleep_score,
	round(avg(systolic)) AS avg_systolic,
	round(avg(Diastolic)) AS avg_diastolic,
	round(avg(Pulse_Pressure)) AS avg_pulse_pressure
FROM SleepLog
GROUP BY sleep_window;
````

<img width="849" height="190" alt="DB_Browser_for_SQLite_CC3bGDQ6sD" src="https://github.com/user-attachments/assets/5e93b9a0-3e15-4ea1-af8f-88d7fb0d13f5" />





### 6. Finding out the minimum, maximum and average sleep quality.
````sql
SELECT count(*) as nights,
	min(Sleep_score) as minimum_sleep_score,
	max(Sleep_score) as maximum_sleep_score,
	avg(Sleep_score) as average_sleep_score
FROM SleepLog;
````

<img width="565" height="66" alt="DB_Browser_for_SQLite_3y2mVPkhfU" src="https://github.com/user-attachments/assets/ea9169db-9ead-434d-9ffe-97086143f420" />


### 7. What is the average sleep duration? Converted from minutes to hours.

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



### 8. Blood pressure and Pulse pressure averages.


````sql
-- average blood pressure
SELECT count(*) as n_nights,
	avg(Systolic) as avg_systolic,
	avg(Diastolic) as avg_diastolic,
	avg(Pulse_Pressure) as avg_pulse_pressure
FROM SleepLog;
````
<img width="445" height="62" alt="DB_Browser_for_SQLite_KUSidLQQRs" src="https://github.com/user-attachments/assets/1ccbb6d2-2271-4301-aa1d-0591fec5835a" />







