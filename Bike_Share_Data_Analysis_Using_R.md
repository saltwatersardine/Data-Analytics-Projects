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

To begin, we must first understand the problem we are trying to solve is the conversion of casual riders into annual members. This will increase the profitability of Cyclistic as annual members have been identified as more profitable than casual riders. The insights derived from this analysis can guide business decisions such as:

1. Designing targeted marketing strategies.
2. Improving the features and services that matter most to casual riders, enhancing their experience and incentivizing them to become annual members.
3. Providing valuable data-backed suggestions for the use of digital media to reach out to and influence casual riders.

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
  - The task is to understand how annual members and casual riders use Cyclistic bikes differently to guide the development of marketing strategies aimed at converting casual riders into annual members.

## Prepare

We will need to ensure that the dataset is clean and well-structured before proceeding with the analysis.

**<ins>Key Tasks</ins>**
* [x] Download data and store it appropriately.
  - Downloaded the previous 12 months of [Cyclistic trip data](https://divvy-tripdata.s3.amazonaws.com/index.html), and saved it in a created folder on my desktop using appropriate file-naming conventions.
* [x] Identify how it’s organized.
  - Once the data is downloaded, I'll load it into RStudio and inspect the structure of the dataset. Look for the number of rows and columns, and understand what each column represents. In this case the all trip data is in comma-delimited (.CSV) format. Column names "ride_id", "rideable_type", "started_at", "ended_at", "start_station_name", "start_station_id", "end_station_name", "end_station_id", "start_lat", "start_lng", "end_lat", "end_lng", "member_casual" (Total 13 column)
* [x] Sort and filter the data.
  - Based on the size of the dataset, the data used for this analysis will be for 2022 (Jan-Dec).
* [x] Determine the credibility of the data.
  - This data is provided by Motivate International Inc, so it should be reliable. Note that because of privacy considerations, the data does not include personally identifiable information. Therefore, some analyses (like connecting pass purchases to credit card numbers) will not be possible.
  - 
**<ins>Deliverable</ins>**
* [x] A description of all data sources used
  - The primary data source is provided by [Cyclistic](https://divvy-tripdata.s3.amazonaws.com/index.html), and it is a publicly available dataset. The data has been made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement).

### Step 1: Load Packages

Start by installing the required packages: `tidyverse`, `lubridate`, `ggplot2`, `dplr` and `readr`

```r
install.packages(c("tidyverse", "lubridate", "ggplot2", "dplyr", "readr", "purrr"))
```

Once a package is installed, you can load it by running the library() function

```r
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(readr)
library(purrr)
```

### Step 2: Importing Data

Use the `file` tab in rstudio to upload the .csv files from my computer. 

<img width="438" alt="file_upload" src="https://user-images.githubusercontent.com/109593672/235165970-56cca0c7-ad34-41a3-a33c-d8561414c62a.png">

Use the `environment` tab to import the data. 

<img width="438" alt="environment_import_dataset" src="https://user-images.githubusercontent.com/109593672/235164985-584d10cf-0173-4b1e-9817-007294f78ba4.png">

Alternatively, click the file name in the file tab and import the dataset. This was done for each .csv from jan22 - dec22

### Step 3: Merging Data 

Merge individual monthly data frames into single file

```r
all_tripdata <- bind_rows(jan22, feb22, mar22, apr22, may22, jun22, jul22, aug22, sep22, oct22, nov22, dec22)
```
### Step 4: Another way to import and merge data.
If you use the code below, you can skip steps 2 and 3. Just make sure you have the `dplyr`, `readr`, and `purrr` libraries installed.

```r
all_tripdata <- 
  list.files(path = "<FILE PATH>", # Used to find all csv files in a folder. Replace <FILE PATH> with the path to your folder.
  pattern = "*.csv",
  full.names = TRUE) %>%   # Gives the complete file path, not just the file name.
  map_df(read_csv)  # Uses the full path to easily find and open each file.
```

## Process
Once we have the data, we need to preprocess it. This may include:

Checking for missing values and handling them appropriately.
Converting data types (e.g., converting `start_time` and `end_time` to datetime objects).
Creating new columns that may be helpful for analysis (e.g., `year`, `month`, `day`, `day_of_week`, `ride_length`).

**<ins>Key Tasks</ins>**
* [x] Check the data for errors.
  - Get to know your data by using the `head()`, `str()`, `colnames()`, and `summary()` functions to view and summarize data.  
* [x] Choose your tools.
  - R and Rstudo (specifically the tidyverse library because of its powerful and user-friendly data manipulation functions)
* [x] Transform the data so you can work with it effectively.
  - Using the `transform` to customise and change columns.  
* [x] Document the cleaning process. 

### Step 1: Inspecting the Data:
Start by checking for missing or inconsistent data. Look for outliers or unusual values, and check the data types of each column to make sure they match what you would expect (e.g., numerical columns should be stored as numbers, dates as date types, etc.)

```r
head(all_tripdata)      # Shows the first several rows. Also tail(all_tripdata).
str(all_tripdata)       # Shows a list of columns and their data types (numeric, character, etc).
colnames(all_tripdata)  # List of column names.
summary(all_tripdata)   # Statistical summary of data - mainly for numerics.
```

### Step 2: Transforming the data
This might involve creating new columns, aggregating data, reshaping the data, or other transformations. For example, we need to aggregate the data by user type (casual riders vs. annual members), and by time (e.g., daily, weekly, monthly). 

**Note:** You should have the `dplyr` library loaded to use the "%>%" operator and the "mutate()" function. The mutate() function is used to replace any inconsistencies in the "member_casual" column. For example, it replaces "Subscriber" with "member" and "Customer" with "casual".  

```r         
new_tripdata <- transform (all_tripdata, 
         year = format(as.Date(started_at), "%Y"),        # Year (4 digit).
         month = format(as.Date(started_at), "%B"),       # Full month.
         day = format(as.Date(started_at), "%d"),         # Decimal date.
         day_of_week = format(as.Date(started_at), "%A"), # Full weekday.
         ride_length = difftime(ended_at, started_at))    # difftime = difference in time between start and end.       
```
`Result:`
<img width="488" alt="colnames" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/a1aa63ad-b9f3-491d-a51a-4930541266de">

**Note:** The separate() function cannot be used here because it includes the time in the day column.

Changing `ride_length` from character to numeric for easier calculations.

```r
new_tripdata$ride_length = as.numeric(as.character(new_tripdata$ride_length))

is.numeric(new_tripdata$ride_length) # This is used to check if it is in the right format.
```
Removing "bad" data from the dataframe. This includes a few hundred entries that involve bikes being taken out of docks for quality checks by Divvy or instances where the ride length is negative.

```r
clean_tripdata <- new_tripdata[!(new_tripdata$ride_length <= 0),]
```
More on date formats in R found here: 
- [https://www.stat.berkeley.edu/~s133/dates.html](https://www.stat.berkeley.edu/~s133/dates.html)
- [https://www.statmethods.net/input/dates.html](https://www.statmethods.net/input/dates.html)

**<ins>Deliverable</ins>**
* [x] Documentation of any cleaning or manipulation of data
  - You should have a clean, well-organized dataset ready for analysis. 

## Analyze
With the data preprocessed, we can now begin analyzing it to answer our questions and generate insights.

**<ins>Key Tasks</ins>**
* [x] Aggregate your data so it’s useful and accessible.
  - Summarizing the data by calculating the mean, median, max, min for both casual riders and annual members.
* [x] Organize and format your data.
* [x] Perform calculations.
* [x] Identify trends and relationships.

### Step 1: Find the Mean, Median, Max and Min

```r
clean_tripdata %>% 
  summarise(average_ride_length = mean(ride_length), 
  median_ride_length = median(ride_length), 
  max_ride_length = max(ride_length), 
  min_ride_length = min(ride_length))
```
`Result:`
<img width="467" alt="Av_length" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/a2851cfd-3a54-4242-93e8-301e6d51e960">

- The data available here represents 'ride_length' for 2022. The smallest and largest ride lengths appear to be unusually disproportionate. They seem way too extreme compared to the rest. We're not sure why yet, but we definitely need to investigate further.

### step 2: Comparing casual riders and annual members 

We will now begin to compare the casual riders with wth annual members 

Note: To use the group_by() and filter() functions, you must first install and load the `dplyr` and `readr` packages.

```r
clean_tripdata %>% 
  group_by(member_casual)%>%
  summarise(number_of_rides = n(), ride_average = (n() / nrow(clean_tripdata)) * 100)
   
ggplot(data = clean_tripdata) +
  geom_bar(mapping = aes(x = member_casual, fill=member_casual)) +
  labs(x="Casual vs Member", y="Number Of Rides", title= "Casuals vs Members distribution")
```
`Result:`
<img width="468" alt="sum_groupby" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/7e915061-6c6a-4d2e-84fe-f878734202e4">
 
`And:`
<img width="501" alt="casual_vs_members_graph" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/7626f39d-c0cc-4ca2-b012-f7c4e7435843">



**<ins>Deliverable</ins>**
* [x] A summary of the analysis

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



























