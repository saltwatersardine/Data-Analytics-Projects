## Introduction

Cyclistic is a fictional bike-share company in Chicago. As a junior data analyst in the marketing team, my goal is to analyze the differences between 'casual' riders and 'annual' members to design a marketing strategy to convert casual riders into annual members. 

This capstone project is part of my [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics)

I will follow the data analysis process: 

- `Ask` 
- `Prepare`
- `Process`
- `Analyze`
- `Share`
- `Act` 

Along the way, the Case Study Roadmap will be used as a guide for each data analysis process. 

* `Code:` when needed
* `Key tasks:` as a checklist
* `Deliverable:` as a checklist

For the analysis I will be using the `R` programming language and `RStudio` for it's easy analysis tools and data visualizations.

## Scenario

You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

The next step is to `Ask` questions that clarify the problem, making sure we completely understand stakeholder expectations.

## Ask

Three questions will guide the future marketing program:

1. How do casual riders and annual members use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

To begin, we must understand the goals of the analysis. In this case, we want to help the Cyclistic attract more riders by analyzing their data and providing insights on how they can improve their services. 

The director of marketing and your manager Lily Moreno has assigned you the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

**<ins>Key Tasks:</ins>**
* [x] Identify the business task
  - The primary business task is to understand how annual members and casual riders use Cyclistic bikes differently. The insights derived from this analysis will be instrumental in guiding marketing strategies aimed at converting casual riders into annual members.
* [x] Consider key stakeholders

  - **Lily Moreno**, the Director of Marketing, who will use the insights to design and implement effective marketing strategies.
  - **The Cyclistic Marketing Analytics Team**, who will assist in data collection, analysis, and reporting.
  - **The Cyclistic Executive Team**, who will make the final decision on the proposed marketing strategies.

**<ins>Deliverable:</ins>**
* [x] A clear statement of the business task
  - Understand how annual members and casual riders use Cyclistic bikes differently to guide the development of marketing strategies aimed at converting casual riders into annual members.

The problem we are trying to solve is the conversion of casual riders into annual members. This will increase the profitability of Cyclistic as annual members have been identified as more profitable than casual riders. The insights derived from this analysis can guide business decisions such as:

1. Designing targeted marketing strategies.
2. Improving the features and services that matter most to casual riders, enhancing their experience and incentivizing them to become annual members.
3. Providing valuable data-backed suggestions for the use of digital media to reach out to and influence casual riders.

Next, we will `Prepare` and `Process` the data to ensure it is clean and well-structured for analysis.

## Prepare

I will use will use [Cyclistic’s historical trip data](https://divvy-tripdata.s3.amazonaws.com/index.html) to analyze and identify trends. The data has been made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement).

**<ins>Key Tasks</ins>**
* [x] Download data and store it appropriately.
  - Downloaded the previous 12 months of [Cyclistic trip data](https://divvy-tripdata.s3.amazonaws.com/index.html), and saved it in a created folder on my desktop using appropriate file-naming conventions.
* [x] Identify how it’s organized.
  - Once the data is downloaded, I'll load it into RStudio and inspect the structure of the dataset. Look for the number of rows and columns, and understand what each column represents. In this case the all trip data is in comma-delimited (.CSV) format. Column names "ride_id", "rideable_type", "started_at", "ended_at", "start_station_name", "start_station_id", "end_station_name", "end_station_id", "start_lat", "start_lng", "end_lat", "end_lng", "member_casual" (Total 13 column)
* [x] Sort and filter the data.
  - Based on the size of the dataset, the data used for this analysis will be from the latest 12 months.
* [x] Determine the credibility of the data.
  - This data is provided by Motivate International Inc, so it should be reliable. Note that because of privacy considerations, the data does not include personally identifiable information. Therefore, some analyses (like connecting pass purchases to credit card numbers) will not be possible.
  - 
**<ins>Deliverable</ins>**
* [x] A description of all data sources used
  - The primary data source is provided by [Cyclistic](https://divvy-tripdata.s3.amazonaws.com/index.html), and it is a publicly available dataset.

### Load Packages

Start by installing the required packages: `tidyverse`, `skimr`, `janitor`

```r
install.packages(c("tidyverse", "skimr", "janitor", "dplyr", "readr", "data.table"))
```

Once a package is installed, you can load it by running the library() function

```r
library(tidyverse)
library(skimr)
library(janitor)
library(dplyr)
library(readr)
library(data.table)
```
I will first use the `file` tab in rstudio to upload the .csv files from my computer. 

<img width="438" alt="file_upload" src="https://user-images.githubusercontent.com/109593672/235165970-56cca0c7-ad34-41a3-a33c-d8561414c62a.png">

You use use the `environment` tab to import the data, or alternatively click the file name in the file tab and import the dataset straight from there.

<img width="438" alt="environment_import_dataset" src="https://user-images.githubusercontent.com/109593672/235164985-584d10cf-0173-4b1e-9817-007294f78ba4.png">

```r
mar22 <- read_csv("divvy_tripdata/202203-divvy-tripdata.csv")
apr22 <- read_csv("divvy_tripdata/202204-divvy-tripdata.csv")
may22 <- read_csv("divvy_tripdata/202205-divvy-tripdata.csv")
jun22 <- read_csv("divvy_tripdata/202206-divvy-tripdata.csv")
jul22 <- read_csv("divvy_tripdata/202207-divvy-tripdata.csv")
aug22 <- read_csv("divvy_tripdata/202208-divvy-tripdata.csv")
sep22 <- read_csv("divvy_tripdata/202209-divvy-tripdata.csv")
oct22 <- read_csv("divvy_tripdata/202210-divvy-tripdata.csv")
nov22 <- read_csv("divvy_tripdata/202211-divvy-tripdata.csv")
dec22 <- read_csv("divvy_tripdata/202212-divvy-tripdata.csv")
jan23 <- read_csv("divvy_tripdata/202301-divvy-tripdata.csv")
feb23 <- read_csv("divvy_tripdata/202302-divvy-tripdata.csv")
mar23 <- read_csv("divvy_tripdata/202303-divvy-tripdata.csv")
```

To start, we need to gather the necessary data. The provided dataset contains the following columns:

- `trip_id`
- `start_time`
- `end_time`
- `bike_id`
- `trip_duration`
- `from_station_id`
- `from_station_name`
- `to_station_id`
- `to_station_name`
- `user_type`
- `gender`
- `birth_year`

We will need to ensure that the dataset is clean and well-structured before proceeding with the analysis.

## Process
Once we have the data, we need to preprocess it. This may include:

Checking for missing values and handling them appropriately.
Converting data types (e.g., converting `start_time` and `end_time` to datetime objects).
Creating new columns that may be helpful for analysis (e.g., `age`, `day_of_week`, `month`, `hour`).

```sql
-- Check for missing values
SELECT COUNT(*) AS missing_values
FROM bike_share_data
WHERE trip_id IS NULL
  OR start_time IS NULL
  OR end_time IS NULL
  OR bike_id IS NULL
  OR trip_duration IS NULL
  OR from_station_id IS NULL
  OR from_station_name IS NULL
  OR to_station_id IS NULL
  OR to_station_name IS NULL
  OR user_type IS NULL
  OR gender IS NULL
  OR birth_year IS NULL;
```

## Analyze
With the data preprocessed, we can now begin analyzing it to answer our questions and generate insights.

## Popular Stations
To find the most popular stations, we can count the number of trips starting and ending at each station.

```sql
-- Most popular starting stations
SELECT from_station_name, COUNT(*) AS num_starts
FROM bike_share_data
GROUP BY from_station_name
ORDER BY num_starts DESC
LIMIT 5;

-- Most popular ending stations
SELECT to_station_name, COUNT(*) AS num_ends
FROM bike_share_data
GROUP BY to_station_name
ORDER BY num_ends DESC
LIMIT 5;
```

## Peak Hours
To find the peak hours for bike usage, we can group trips by the hour and count the number of trips.

```sql
-- Peak hours
SELECT EXTRACT(HOUR FROM start_time) AS hour, COUNT(*) AS num_trips
FROM bike_share_data
GROUP BY hour
ORDER BY num_trips DESC;
```

## Rider Behavior
To analyze rider behavior based on the day of the week or season, we can group trips by these variables and compare the results.

```sql
-- Trips by day of the week
SELECT EXTRACT(DOW FROM start_time) AS day_of_week, COUNT(*) AS num_trips
FROM bike_share_data
GROUP BY day_of_week
ORDER BY day_of_week;

-- Trips by month
SELECT EXTRACT(MONTH FROM start_time) AS month, COUNT(*) AS num_trips
FROM bike_share_data
GROUP BY month
ORDER BY month;
```
Next, we can analyze the average trip duration by user type, gender, and age group to understand how different demographics use the service.

```sql
-- Average trip duration by user type
SELECT user_type, AVG(trip_duration) AS avg_trip_duration
FROM bike_share_data
GROUP BY user_type;

-- Average trip duration by gender
SELECT gender, AVG(trip_duration) AS avg_trip_duration
FROM bike_share_data
WHERE gender IS NOT NULL
GROUP BY gender;

-- Average trip duration by age group
SELECT
  CASE
    WHEN EXTRACT(YEAR FROM AGE(NOW(), birth_year)) < 30 THEN 'Under 30'
    WHEN EXTRACT(YEAR FROM AGE(NOW(), birth_year)) BETWEEN 30 AND 59 THEN '30-59'
    ELSE '60 and over'
  END AS age_group,
  AVG(trip_duration) AS avg_trip_duration
FROM bike_share_data
WHERE birth_year IS NOT NULL
GROUP BY age_group;
```

## Share
Based on our analysis, we can provide the following insights for the bike-share company:

1. The most popular stations, both for starting and ending trips.
2. The peak hours for bike usage, which can be useful for managing bike availability and maintenance schedules.
3. Rider behavior trends, such as variations in trip volume based on the day of the week or season, and differences in average trip duration by user type, gender, and age group.

These insights can help the company identify areas for improvement and potential opportunities for attracting more riders.

## Act
Based on the insights gained from our analysis, the bike-share company could consider the following actions:

1. Increase the number of bikes and docking stations at popular locations to meet demand.
2. Optimize bike maintenance schedules to ensure maximum bike availability during peak hours.
3. Develop targeted marketing campaigns to attract specific demographics, such as offering discounts for certain user types or promoting the service to underrepresented age groups.
4. Investigate further into the factors influencing ridership trends, such as weather conditions, local events, or pricing structures.

By implementing these recommendations, the company can improve its services and attract more riders, ultimately leading to increased revenue and growth.



























