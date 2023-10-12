# CyclisticDA-GDA
Google Data Analyst Capstone Project

---
title: "Google Data Analytics - Cyclistic"
author: "Raphael"
date: "2023-10-10"
output:
  html_document: default
  pdf_document: default
---

# Case Study 1: How Does a Bike-Share Navigate Speedy Success?

## Introduction

Cyclistic is a bike-share program that offers 5,800 bicycles and 600 docking stations across Chicago. They also offer reclining bikes, hand tricycles, and cargo bikes to make more inclusive people with disablities. While common riders opt to traditional bikes, 8% of riders opt to using assistive options. Cyclistic is popular for leisure acitivities across Chicago, 30% of users use it as a communiting alternative for work. Cyclistic marketing team aims to increase their profit by converting casual riders to annual members. By analyzing the historical bike trips to  identify trends. The team envisions to understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing strategy.

## Scenario

As a junior data analyst working with the marketing team of Cyclistic, The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Your team is task to to understand how casual riders and annual members use Cyclistic bikes differently. With the brainstormed insight from your team, you will be building a new marketing strategy to convert casual riders into annual members and come up with recommendation that are solid and backed up, with compelling data insights and professional data visualizations.

## Data Analysis Process 
In this case study your objectives are to produce a compelling report of the following deliverables:

1. A clear statement of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top three recommendations based on your analysis

By utilizing the 6 fundatamentals of Google data analystics: 

1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

### Phase 1: Act - A clear statement of the business task

**Key Task**: Identify the business task by considering the key stakeholders goals, through the following questions for the future marketing program for anaylsis: 

1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

As a juinor data analyst my manager task me to team with the Marketing team to understand "How casual riders and annual members use Cyclistic bikes differently"?

### Phase 2: Prepare - A description of all data sources used

**Key Task**: a.) Clean the Data through filtering, sorting, and organizing. b.) Cite the credibility of the Data

**Data Source**: For the analyzation and identification of trends, Jan 2022 to Dec 2022 Cyclistic's data trip will be utilized. [Click Here!](https://divvy-tripdata.s3.amazonaws.com/index.html) for the dataset. Open-data source made available by Motivate International Inc. under [liscense](https://divvybikes.com/data-license-agreement). 

**Data Information**: The data being worked with contains an entire year of data. With naming format convention of *"YYYYMM-divvy-tripdata"*. Each file is compose of monthly data that encompasses with the following details as columns:

``` 
colnames(X202201_divvy_tripdata)

[1] "ride_id"            "rideable_type"      "started_at"        
[4] "ended_at"           "start_station_name" "start_station_id"  
[7] "end_station_name"   "end_station_id"     "start_lat"         
[10] "start_lng"          "end_lat"            "end_lng"           
[13] "member_casual"     
```
### Phase 3: Process - Documentation of any cleaning or manipulation of data

**Data Tool**: Google BigQuery is used to load and combine data of 12 months into 1 data set for Cyclistic's data trip

#### Merging of data

12 CSV files has been uploaded and created as tables in the BigQuery. For a more organized analyzation the data are merged into one dataset. The data set is named "2022_tripdata", Under the new data set, a new table is created. Within it, is the 12 dataset from the months of year 2022 named ***"year22_data"*** and is generated with the data code: 

```
CREATE TABLE IF NOT EXISTS
  `2022_tripdata.year22_data` AS 
  (
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.jan_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.feb_tripdata`
    UNION ALL
    SELECT * FROM  `my-data-project-1-399301.2022_tripdata.mar_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.apr_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.may_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.jun_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.jul_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.aug_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.sep_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.oct_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.nov_tripdata`
    UNION ALL
    SELECT * FROM `my-data-project-1-399301.2022_tripdata.dec_tripdata`
  )
```
After combining all the data from year 2022, I run I query to double check the number of row in the ***"year22_data"*** which resulted to: 
```
SELECT COUNT(*) AS Record_Total_2022
FROM `2022_tripdata.year22_data`
```
![](https://shorturl.at/izKLN)

### Processing and Organizing the Data

1. In the first step of processing the data, I verified the credibility of it, by checking it's metadata on the schema.
```
SELECT * 
FROM `2022_tripdata`.INFORMATION_SCHEMA.COLUMNS
```
![](https://shorturl.at/euQZ7)

2. Next is I checked for possible null values in the columns for data cleaning purposes.

```
SELECT 
COUNT(*) - COUNT(ride_id) ride_id,
 COUNT(*) - COUNT(rideable_type) rideable_type,
 COUNT(*) - COUNT(started_at) started_at,
 COUNT(*) - COUNT(ended_at) ended_at,
 COUNT(*) - COUNT(start_station_name) start_station_name,
 COUNT(*) - COUNT(start_station_id) start_station_id,
 COUNT(*) - COUNT(end_station_name) end_station_name,
 COUNT(*) - COUNT(end_station_id) end_station_id,
 COUNT(*) - COUNT(start_lat) start_lat,
 COUNT(*) - COUNT(start_lng) start_lng,
 COUNT(*) - COUNT(end_lat) end_lat,
 COUNT(*) - COUNT(end_lng) end_lng,
 COUNT(*) - COUNT(member_casual) member_casual
FROM `2022_tripdata.year22_data`
```
In the results there where null values among the columns: 

* start_station_name = 833064
* start_station_id = 833064
* end_station_name = 892742
* end_station_id = 892742
* end_lat = 5858
* end_lng = 5858


3. After distinguising the null values in the data. I then continue to clean the data by identifying duplicate rows using the ***ride_id*** column.
```
SELECT COUNT(ride_id) - COUNT(DISTINCT ride_id) AS duplicate_rows 
FROM `2022_tripdata.year22_data`;
```
So far there are no duplicate rows or entries.

4. Next is to pull the records of ***rideable_type*** offer of Cyclistic to identify the total rides from the 3 different bike types:
```
SELECT DISTINCT rideable_type, COUNT(rideable_type) AS total_trip_type
FROM `2022_tripdata.2022clean_tripdata`
GROUP BY rideable_type
```
![](https://shorturl.at/apsw3)

6. And then I pulled the data between the casual and member riders to identify the total rides from the 2 types of riders:
```
SELECT DISTINCT member_casual, 
  COUNT(*) AS count_member_type
FROM `2022_tripdata.2022clean_tripdata`
GROUP BY member_casual
```
![](https://shorturl.at/yAIW7)

7. In the columns **"started_at"** and **"ended_at"** the format is indicated as YYYY-DD-MM hh:mm:ss UTC. I just look into 10 dataset to easiily look into this columns
```
SELECT started_at, ended_at -- started_at, ended_at - TIMESTAMP - YYYY-MM-DD hh:mm:ss UTC
FROM `2022_tripdata.2022clean_tripdata`
LIMIT 10;
```
8. In order to analyze the data further creating a new column named ***"ride_length"*** which calculates the duration of each ride by subtracting the "started_at" and "ended_at" columns will help to formulate insights. 

In addition to adding columns, to calculate for the day of the week that each ride started, ***"day_of_week"*** and ***"month"*** is also added

9. There were 5,360 trips that had a duration longer than a day
```
SELECT COUNT(*) AS longer_than_a_day
FROM `2022_tripdata.year22_data`
WHERE (
  EXTRACT(HOUR FROM (ended_at - started_at)) * 60 +
  EXTRACT(MINUTE FROM (ended_at - started_at)) +
  EXTRACT(SECOND FROM (ended_at - started_at)) / 60) >= 1440;   
```
![](https://shorturl.at/ozGMW)

10. There were 122,283 trips that had a earlier end_time than start_time.

```
SELECT COUNT(*) AS less_than_a_minute
FROM `2022_tripdata.year22_data`
WHERE (
  EXTRACT(HOUR FROM (ended_at - started_at)) * 60 +
  EXTRACT(MINUTE FROM (ended_at - started_at)) +
 EXTRACT(SECOND FROM (ended_at - started_at)) / 60) <= 1;    
 ```
![](https://shorturl.at/ipsQY)

11. Rows that have null values from columns start_station_name and start_station_id must be remove. A total of 833,064 row must be cleaned for the integrity of data
```
SELECT DISTINCT start_station_name
FROM `2022_tripdata.year22_data`
ORDER BY start_station_name;

SELECT COUNT(ride_id) AS rows_with_start_station_null         
FROM `2022_tripdata.year22_data`
WHERE start_station_name IS NULL OR start_station_id IS NULL;
```

12. Rows that have null values from columns end_station_name and end_station_id must be remove. A total of 892742 row must be cleaned for the integrity of data
```
SELECT DISTINCT end_station_name
FROM `2022_tripdata.year22_data`
ORDER BY end_station_name;

SELECT COUNT(ride_id) AS rows_with_null_end_station          
FROM `2022_tripdata.year22_data`
WHERE end_station_name IS NULL OR end_station_id IS NULL;
```

13. There are null values among longtitude and latitude but this values might not contribute to the analysis of data but instead help in the visualization of region map.

### Cleaning the data 

1. Created a new data table for the cleaned dataset were all null values were removed.
2. There were three columns added which are: 

* ***"ride_length"*** for duration of trip
* ***"day_of_week"*** for the weeks in a month
* ***"month"*** for the months in the year 2022
3. Ride trips with duration labeled as less_than_a_minute and longer_than_a
_day are disregarded.

4. From the original 5,667,717 values in the data set, after cleaning there were only 4,291,805 remaining which means there were 1,375,912 null values removed. 

```
CREATE TABLE IF NOT EXISTS `2022_tripdata.cleaned_year22_data` AS (
  SELECT 
    a.ride_id, rideable_type, started_at, ended_at, 
    ride_length,
    CASE EXTRACT(DAYOFWEEK FROM started_at) 
      WHEN 1 THEN 'SUN'
      WHEN 2 THEN 'MON'
      WHEN 3 THEN 'TUES'
      WHEN 4 THEN 'WED'
      WHEN 5 THEN 'THURS'
      WHEN 6 THEN 'FRI'
      WHEN 7 THEN 'SAT'    
    END AS day_of_week,
    CASE EXTRACT(MONTH FROM started_at)
      WHEN 1 THEN 'JAN'
      WHEN 2 THEN 'FEB'
      WHEN 3 THEN 'MAR'
      WHEN 4 THEN 'APR'
      WHEN 5 THEN 'MAY'
      WHEN 6 THEN 'JUN'
      WHEN 7 THEN 'JUL'
      WHEN 8 THEN 'AUG'
      WHEN 9 THEN 'SEP'
      WHEN 10 THEN 'OCT'
      WHEN 11 THEN 'NOV'
      WHEN 12 THEN 'DEC'
    END AS month,
    start_station_name, end_station_name, 
    start_lat, start_lng, end_lat, end_lng, member_casual
  FROM `2022_tripdata.year22_data` a
  JOIN (
    SELECT ride_id, (
      EXTRACT(HOUR FROM (ended_at - started_at)) * 60 +
      EXTRACT(MINUTE FROM (ended_at - started_at)) +
      EXTRACT(SECOND FROM (ended_at - started_at)) / 60) AS ride_length
    FROM `2022_tripdata.year22_data`
  ) b 
  ON a.ride_id = b.ride_id
  WHERE 
    start_station_name IS NOT NULL AND
    end_station_name IS NOT NULL AND
    end_lat IS NOT NULL AND
    end_lng IS NOT NULL AND
    ride_length > 1 AND ride_length < 1440
);

ALTER TABLE `2022_tripdata.cleaned_year22_data`     -- set ride_id as primary key
ADD PRIMARY KEY(ride_id) NOT ENFORCED;

SELECT COUNT(ride_id) AS no_of_rows       -- returned 4,291,805 rows so 1,375,912 rows removed
FROM `2022_tripdata.cleaned_year_data`;
```

### Phase 4 and 5: Analyze and Share Data

After thoroughly cleaning the data for the analysis process, the most relevant dataset and from multiple tables is then exported and will be used for analyzation using Tableau and R. The main goal in this phase is to create a summary of analysis in identifying trends and relationship in order to understand how casual riders and annual members use Cyclistic bikes differently. 

1. To further widen our perspective on the subject. The analyzation and comparison between the casual riders and annual members in terms of the usage of what bike type is more favored.

```
SELECT member_casual, rideable_type, COUNT(*) AS total_trips
FROM `2022_tripdata.cleaned_combined_data`
GROUP BY member_casual, rideable_type
ORDER BY member_casual, total_trips;
```

![](https://shorturl.at/vMPSX)

Based on the comparison among casual and member riders:

* In pie chart of classic bike - annual members tend to use more of the classic bikes.
* In the pie chart of docked bike - only casual riders use docked bikes because of leisure purposes.
* In the pie chart of electric bike - most member riders use electric bike most often for commuting purposes.

**The Grand Total:** Annual members has more bike trips recorded than casual riders 

2. Another key insight to help understood the goal of the team is to analyze the total trips per month, week, and hour among riders 

```
-- total trips per month among riders 
SELECT month, member_casual, 
  COUNT(ride_id) AS total_trips
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY month, member_casual
ORDER BY member_casual;

-- total trips per week among riders
SELECT day_of_week, member_casual, 
  COUNT(ride_id) AS total_trips
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY day_of_week, member_casual
ORDER BY member_casual;

-- total trips per hour among riders
SELECT 
  EXTRACT(HOUR FROM started_at) 
  AS hour_of_day, member_casual, 
  COUNT(ride_id) AS total_trips
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY hour_of_day, member_casual
ORDER BY member_casual;
```
![](https://shorturl.at/uOQ37)

* **Month:** Analyzing the viz of the monthly trips, there is a similar pattern of the trips recorded. The occurence of trips starts to rise from Srping until the End of the summer. Were the peak of usage is during summer, and its start do decline as it approaches winter season. In addition, there are more annual members than casual riders.

* **Week:** Comparing the viz of weekly trips, It's obvious to see that there are more bike trips from casual rider from the weekend as it is the time fore leisure. While there is a declining trend of usage from the annual members as it is their restday from their works.

* **Hour:** Looking at the viz of hourly trips, there is a spike of trips in the morning starting from 6:00 until 8:00 and in the afternoon from 14:00 to 17:00 specifically among annual members. However, for the casual riders, there is a relatively increasing trend of trips in the morning 'til afternoon.

With these observations, we can assume that annual members use bikes for commuting daily to work during weekday, while casual riders use bikes for leisure puposes on weekends. Taking insights from the monthly viz, there is a relativity of usage during spring to summer among the riders. 

3. After considering the *number of trips* within months, weeks, and hour. Another helpful insight to further understand the goal is to look for the data of *ride length or duration* among riders. 

```
-- average ride_length per month in mins
SELECT month, member_casual, 
  AVG(ride_length) 
  AS avg_ride_duration
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY month, member_casual;

-- average ride_length per week in mins
SELECT day_of_week, member_casual, 
AVG(ride_length) 
AS avg_ride_duration
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY day_of_week, member_casual;

-- average ride_length per hr in mins
SELECT EXTRACT(HOUR FROM started_at) 
AS hour_of_day, member_casual, 
AVG(ride_length) 
AS avg_ride_duration
FROM `2022_tripdata.cleaned_year22_data`
GROUP BY hour_of_day, member_casual;
```
![](https://shorturl.at/efDNV)

Based on the findings of the data and the result of the data visualization, By looking at the average duration casual riders have longer biking trips than the annual members. 

There is a consistent trend of bike duration among the annual member this is because they ride bikes for commuting to get to work. While looking at the trend of casual riders, there is a consistent trend of longer bike trips, this is because casual riders use the bike for leisure purposes. 

Some points to consider, casual riders have an increasing trend of users and longer bike trips during spring until summer at weekends and from time of 10:00 to 14:00. While for annual members there is a straight consistent trend thoughout the months, weeks, and hours. 

4. To further, gain insight and visaulize the scenario of the goal which is to understand the differences of casual riders and annual members. The analysis of the start and end stations across the map is an addition for the key insights to achieve the goal. 

![](https://shorturl.at/cuyGL)

Looking and examining closer to the map we can assume that casual riders tend to start and end their trips from the stations that are near from museums, aquarium parks, harbors, beaches, parks, and plazas. While annual members, tend to start and end their journey from stations near to residential areas, train stations, factories, schools, universities, restaurants, hospitals, and malls.

![](https://shorturl.at/kquBO)

With the data points across the map we can add into conclusion that *casual riders* start and end their trips near leisure areas. While *annual members* primary use bikes for commuting as a reliable mode of transporation to get to work. Which supports the underlying statement from the problem scenario.

#### **Executive Summary**
![](https://shorturl.at/bfor7)

### Phase 6: Act - Recommend 

With the thorough analysis process with the given data, the execution and identification of key insights to meet the goal of team to create a new marketing program that understands **how do annual members and casual riders use Cyclistic bikes differently?**, distinguish **why would casual riders buy Cyclistic annual memberships** ,and **how can Cyclistic use digital media to influence casual riders to become members** resulto to the following:

**Recommendations:**

1. Initiate marketing campaigns before the start of spring and summer across tourist spots in the region were casual riders take interest in the offerings of Cyclistics.
2. With the support of the evidence that most casual riders take the offering of bikes from Cyclistic during weekends, spring ,and summer seasons, cyclistic marketing should offer vouchers, seasonal and weekly promotions.
3. Though casual riders tend to have longer ride length/duration, the marketing team should provide discount incentive for extended usage of bikes to casual members to further encourage the casual riders to convert to annual membership.






