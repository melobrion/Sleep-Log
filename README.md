# Personal Sleep Log and Blood Pressure

## Questions

### 1. What is the average sleep time during this time period?

''''sql
SELECT count(*) AS n_nights
FROM SleepLog;


### 1. What is the average sleep time during this time period?

````sql
SELECT *
FROM SleepLog
WHERE Sleep_Time BETWEEN  '23:00' AND '23:59';
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




