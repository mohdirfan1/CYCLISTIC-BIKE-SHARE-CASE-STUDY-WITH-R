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


Convert ride_id and rideable_type to character so that they can stack correctly
```
q4_2019 <-  mutate(q4_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 
q3_2019 <-  mutate(q3_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 
q2_2019 <-  mutate(q2_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 

```


Stack individual quarter's data frames into one big data frame
```
all_trips <- bind_rows(q2_2019, q3_2019, q4_2019, q1_2020)
```
Remove lat, long, birthyear, and gender fields as this data was dropped beginning in 2020
```
all_trips <- all_trips %>%  
  select(-c(start_lat, start_lng, end_lat, end_lng, birthyear, gender, "01 - Rental Details Duration In Seconds Uncapped", "05 - Member Details Member Birthday Year", "Member Gender", "tripduration"))
```
### CLEAN UP AND ADD DATA TO PREPARE FOR ANALYSIS

```
summary(all_trips)  #Statistical summary of data. Mainly for numerics

```
# There are a few problems we will need to fix:
(1) In the "member_casual" column, there are two names for members ("member" and "Subscriber") and two names for casual riders ("Customer"         and "casual"). We will need to consolidate that from four to two labels.
(2) The data can only be aggregated at the ride-level, which is too granular. We will want to add some additional columns of data -- such as       day, month, year -- that provide additional opportunities to aggregate the data.
(3) We will want to add a calculated field for length of ride since the 2020Q1 data did not have the "tripduration" column. We will add       
      "ride_length" to the entire dataframe for consistency.
(4) There are some rides where tripduration shows up as negative, including several hundred rides where Divvy took bikes out of circulation         for Quality Control reasons. We will want to delete these rides.

In the "member_casual" column, replace "Subscriber" with "member" and "Customer" with "casual"
Before 2020, Divvy used different labels for these two types of riders ... we will want to make our dataframe consistent with their current nomenclature
N.B.: "Level" is a special property of a column that is retained even if a subset does not contain any values from a specific level

***Begin by seeing how many observations fall under each usertype***
```
table(all_trips$member_casual)
```

Reassign to the desired values (we will go with the current 2020 labels)
```
all_trips <-  all_trips %>% 
  mutate(member_casual = recode(member_casual
                           ,"Subscriber" = "member"
                           ,"Customer" = "casual"))
```

 Check to make sure
```
table(all_trips$member_casual)
```
***result***
![Screenshot (169)](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/eaf252bb-4a4e-4886-b848-bc378e911794)

Add columns that list the date, month, day, and year of each ride
```
all_trips$date <- as.Date(all_trips$started_at) #The default format is yyyy-mm-dd
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")


```

Add a "ride_length" calculation to all_trips (in seconds)
```
all_trips$ride_length <- difftime(all_trips$ended_at,all_trips$started_at)
```
Inspect the structure of the columns
```
str(all_trips)
```
**result**

![Screenshot (171)](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/975eb82a-dde3-4be7-a295-a81668f18f81)

**Remove "bad" data**
The dataframe includes a few hundred entries when bikes were taken out of docks and checked for quality by Divvy or ride_length was negative

*We will create a new version of the dataframe (v2) since data is being removed*
```
all_trips_v2 <- all_trips[!(all_trips$start_station_name == "HQ QR" | all_trips$ride_length<0),]
```

 ### CONDUCT DESCRIPTIVE ANALYSIS

 Descriptive analysis on ride_length (all figures in seconds)
 ```
mean(all_trips_v2$ride_length) #straight average (total ride length / rides)
median(all_trips_v2$ride_length) #midpoint number in the ascending array of ride lengths
max(all_trips_v2$ride_length) #longest ride
min(all_trips_v2$ride_length) #shortest ride
```
You can condense the four lines above to one line using summary() on the specific attribute
```
summary(all_trips_v2$ride_length)
```
![Screenshot (173)](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/b91f51f4-fa45-43a3-9303-f3b16146d1f5)


See the average ride time by each day for members vs casual users\ \
``
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
``



![Screenshot (175)](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/7df7dc4d-9868-4fca-af9c-a7ea191da790)

Now, let's run the average ride time by each day for members vs casual users\\

```
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
```

![Screenshot (177)](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/9f17a24f-67e0-4a12-b84d-4efe8dc4935c)

# Let's visualize the number of rides by rider type

```
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge")
```

![Rplot01](https://github.com/mohdirfan1/CYCLISTIC-BIKE-SHARE-CASE-STUDY-WITH-R/assets/107385987/3bef17c9-2444-4ef9-8c5c-ebcbaca5fca1)


