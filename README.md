# CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R

### INTRODUCTION
This capstone project is the final project in my Google Data Analytics Professional Certificate course. In this case study I will be analyzing a public dataset for a fictional company provided by the course. I will be using R programming language for this analysis because of its easy statistical analysis tools and data visualizations.

### The following data analysis steps will be followed:

* Ask,
* Prepare,
* Process,
* Analyze,
* Share,
* Act.

### The case study roadmap below will be followed on each step:
* Code, when needed.
* tasks.
* Deliverables.

  
### Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore,your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

### ASK
* Three questions will guide the future marketing program:\
1.How do annual members and casual riders use Cyclistic bikes differently?\
2.Why would casual riders buy Cyclistic annual memberships?\
3.How can Cyclistic use digital media to influence casual riders to become members? 
Lily Moreno (the director of marketing and my manager) has assigned you the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

### Key tasks
*Identify the business task\
* The main objective is to design marketing strategies aimed at converting casual riders to annual members by\    *understanding how they differ.\
*  Consider key stakeholders\
* Director of Marketing (Lily Moreno), Marketing Analytics team, Executive team

### Deliverable
A clear statement of the business task\
To find the differences between the casual riders and annual members.\

### PREPARE
I will use Cyclistic’s historical trip data to analyze and identify trends. The data has been made available by Motivate International Inc. under this license. Datasets are available here link.
Key tasks\
1.Download data and store it appropriately.\
Data has been downloaded and copies have been stored securely on my computer and here on Kaggle.\
2. Identify how it’s organized.\
The data is in CSV (comma-separated values) format, and there are a total of 13 columns.\
3. Sort and filter the data.\
For this analysis, I will be using data for the last 12 months (Feb 2021 - Jan 2022) because it is more current.\
4. Determine the credibility of the data.\
For the purpose of this case study, the datasets are appropriate and will enable me to answer the business questions. But data-privacy issues will prohibit me from using rider's personally identifiable information and this will prevent me from determining if riders have purchased multiple single passes. All ride ids are unique

### Deliverable

A description of all data sources used
Main source of data provided by the **Cylistic company.**\
Install and load necessary packages

### Install and load necessary packages
```

library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
```

### Import data to R studio
```
q1_2019 <- read.csv("D:/Case_Study/Case_study_1/Data/Extract_data/Divvy_Trips_2019_Q1.csv")
q2_2019 <- read.csv("D:/Case_Study/Case_study_1/Data/Extract_data/Divvy_Trips_2019_Q2_1.csv")
q3_2019 <- read.csv("D:/Case_Study/Case_study_1/Data/Extract_data/Divvy_Trips_2019_Q3.csv")
q4_2019 <- read.csv("D:/Case_Study/Case_study_1/Data/Extract_data/Divvy_Trips_2019_Q4.csv")
```

###  WRANGLE DATA AND COMBINE INTO A SINGLE FILE
Compare column names each of the files
While the names don't have to be in the same order, they DO need to match perfectly before we can use a command to join them into one file
```
colnames(q3_2019)
colnames(q4_2019)
colnames(q2_2019)
colnames(q1_2019)
```

Rename columns  to make them consistent with q1_2019 (as this will be the supposed going-forward table design for Divvy)

```
(q1_2019 <- rename(q1_2019
                   ,ride_id = trip_id
                   ,rideable_type = bikeid 
                   ,started_at = start_time  
                   ,ended_at = end_time  
                   ,start_station_name = from_station_name 
                   ,start_station_id = from_station_id 
                   ,end_station_name = to_station_name 
                   ,end_station_id = to_station_id 
                   ,member_casual = usertype))

(q2_2019 <- rename(q2_2019
                   ,ride_id = Rental_ID
                   ,rideable_type = Rental_Details_Bike_ID
                   ,started_at = Rental_Details_Local_Start_Time  
                   ,ended_at = Rental_Details_Local_End_Time  
                   ,start_station_name = Rental_Start_Station_Name 
                   ,start_station_id = Rental_Start_Station_ID 
                   ,end_station_name = Rental_End_Station_Name
                   ,end_station_id = Rental_End_Station_ID
                   ,member_casual = User_Type ))
(q3_2019 <- rename(q3_2019
                   ,ride_id = trip_id
                   ,rideable_type = bikeid 
                   ,started_at = start_time  
                   ,ended_at = end_time  
                   ,start_station_name = from_station_name 
                   ,start_station_id = from_station_id 
                   ,end_station_name = to_station_name 
                   ,end_station_id = to_station_id 
                   ,member_casual = usertype))
(q4_2019 <- rename(q4_2019
                   ,ride_id = trip_id
                   ,rideable_type = bikeid 
                   ,started_at = start_time  
                   ,ended_at = end_time  
                   ,start_station_name = from_station_name 
                   ,start_station_id = from_station_id 
                   ,end_station_name = to_station_name 
                   ,end_station_id = to_station_id 
                   ,member_casual = usertype))
```

Compare column names each of the files\
 While the names don't have to be in the same order, they DO need to match perfectly before we can use a command to join them into one file
```{r compare column names }

colnames(q3_2019)
colnames(q4_2019)
colnames(q2_2019)
colnames(q1_20

```
