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

**<ins>Deliverable</ins>**
* [x] A description of all data sources used
  - The primary data source is provided by [Cyclistic](https://divvy-tripdata.s3.amazonaws.com/index.html), and it is a publicly available dataset. The data has been made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement).

### Step 1: Load Packages

Start by installing the required packages: `tidyverse`, `lubridate`, `ggplot2`, `dplr` and `readr`

```r
install.packages(c("tidyverse", "lubridate", "ggplot2", "dplyr", "readr", "purrr", "goesphere"))
```

Once a package is installed, you can load it by running the library() function

```r
library(tidyverse)
library(lubridate)
library(ggplot2)
library(dplyr)
library(readr)
library(purrr)
library(geosphere)
```

### Step 2: Importing Data

Use the `file` tab in rstudio to upload the .csv files from my computer. 

<div>
  <img width="438" alt="file_upload" src="https://user-images.githubusercontent.com/109593672/235165970-56cca0c7-ad34-41a3-a33c-d8561414c62a.png">
</div>
<br>

Use the `environment` tab to import the data.

<div>
  <img width="438" alt="environment_import_dataset" src="https://user-images.githubusercontent.com/109593672/235164985-584d10cf-0173-4b1e-9817-007294f78ba4.png">
</div>
<br>
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

_**Note:**_ You should have the `dplyr` library loaded to use the "%>%" operator and the "mutate()" function. The mutate() function is used to replace any inconsistencies in the "member_casual" column. For example, it replaces "Subscriber" with "member" and "Customer" with "casual".  

```r         
new_tripdata <- transform (all_tripdata, 
         year = format(as.Date(started_at), "%Y"),        # Year (4 digit).
         month = format(as.Date(started_at), "%B"),       # Full month.
         day = format(as.Date(started_at), "%d"),         # Decimal date.
         day_of_week = format(as.Date(started_at), "%A"), # Full weekday.
         ride_length = difftime(ended_at, started_at),    # difftime = difference in time between start and end.
         start_time = strftime(started_at, "%H"))           
```
`Result:`
<div>
<img width="488" alt="colnames" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/a1aa63ad-b9f3-491d-a51a-4930541266de">
</div>
<br>

_**Note:**_ The separate() function cannot be used here because it includes the time in the day column.

Changing `ride_length` from character to numeric for easier calculations.

```r
new_tripdata$ride_length = as.numeric(as.character(new_tripdata$ride_length))

is.numeric(new_tripdata$ride_length) # This is used to check if it is in the right format.
```
Adding ride distance in km:
```r
new_tripdata$ride_distance <- distGeo
# Measures the distance between the start and end coordinates of each trip using distGeo() from the geosphere package.

(matrix(c(new_tripdata$start_lng, new_tripdata$start_lat), ncol = 2), matrix(c(new_tripdata$end_lng, new_tripdata$end_lat), ncol = 2))
# This code creates a matrix of start and end coordinates and calculates the geographical distance between them. 

new_tripdata$ride_distance <- new_tripdata$ride_distance/1000 #distance in km
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

**<ins>Deliverable</ins>**
* [x] A summary of the analysis

### Step 1: Find the Mean, Median, Max and Min

```r
clean_tripdata %>% 
  summarise(average_ride_length = mean(ride_length), 
  median_ride_length = median(ride_length), 
  max_ride_length = max(ride_length), 
  min_ride_length = min(ride_length))
```
`Result:`
<br>
<div>
<img width="467" alt="Av_length" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/a2851cfd-3a54-4242-93e8-301e6d51e960">
</div>

The data available here represents 'ride_length' for 2022. The smallest and largest ride lengths appear to be unusually disproportionate. They seem way too extreme compared to the rest. We're not sure why yet, but we definitely need to investigate further.

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
<br>
<div>
<img width="468" alt="sum_groupby" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/7e915061-6c6a-4d2e-84fe-f878734202e4">
</div>
<br>
<div>
<img width="300" alt="casual_vs_members_graph" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/2ab0edb2-693f-4343-a8c4-e20bae3378d3">
</div>

In 2022, members used ride-sharing services 18% more than casual riders, as shown in the distribution chart where members make up 59% and casual riders make up 41% of the dataset.

**The comparison between member and casual riders is made based on ride length, considering metrics such as mean, median, minimum, and maximum values.**

```r
clean_tripdata %>%
  group_by(member_casual) %>% 
  summarise(average_ride_length = mean(ride_length), median_length = median(ride_length), 
            max_ride_length = max(ride_length), min_ride_length = min(ride_length))
```
`Result:`

<div>
  <img width="621" alt="average_comparision" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/1d51a188-8127-4b0a-aa0a-c2daff4adabe">
</div>
<br>

Based on the table above, it's clear that casual riders tend to take longer rides than members. This is because the average trip duration or ride length for casual riders is higher compared to that of member riders.

**Average ride times for members and casual users, listed in order by each day of the week.**
```r
clean_tripdata$day_of_week <- ordered(clean_tripdata$day_of_week, 
                                    levels=c("Sunday","Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```
**Analyze ridership data by type and weekday.**
```r
clean_tripdata %>% 
  group_by(member_casual, day_of_week) %>%  #groups by member_casual
  summarise(number_of_rides = n(), #calculates the number of rides and average duration 
  average_ride_length = mean(ride_length)) %>% # calculates the average duration
  arrange(member_casual, day_of_week)
```
`Result:`

<div>
  <img width="623" alt="groups_2" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/f9690df9-a9bf-4092-9756-1f8e4c8e0580">
</div>
<br>

**visualize the number of rides by rider type.**
```r
clean_tripdata %>%  
  group_by(member_casual, day_of_week) %>% 
  summarise(number_of_rides = n(), .groups="drop") %>% 
  arrange(member_casual, day_of_week)  %>% 
  ggplot(aes(x = day_of_week, y = number_of_rides, fill = member_casual)) +
  labs(title ="Total Rides: Members vs. Casual Riders by Day") +
  geom_col(position = "dodge") +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

`Result:`

<div>
  <img width="563" alt="Total_rides_vis" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/8a55208c-d4c6-4a10-9413-dfe34691ef4c">
</div>
<br>

The chart shows that members had consistent trip numbers throughout the week, while casual riders had the most rides on weekends, with a peak on Saturday.

**Visualize average ride time data by type and day of week.**
```r
clean_tripdata %>%  
  group_by(member_casual, day_of_week) %>% 
  summarise(average_ride_length = mean(ride_length), .groups="drop") %>%
  ggplot(aes(x = day_of_week, y = average_ride_length, fill = member_casual)) +
  geom_col(position = "dodge") + 
  theme(axis.text.x = element_text(angle = 30)) +
  labs(title ="Average Ride Time: Members vs. Casual Riders by Day")
```
`Result:`

<div>
  <img width="563" alt="Average_ride_time" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/75b70aaa-e137-40fd-9c8f-0013003218ec">
</div>
<br>

As seen above, members typically have shorter average ride lengths than casual riders. Casual riders, on the other hand, tend to have higher average ride lengths and more rides on weekends. This indicates a connection between longer rides and weekends for casual riders. In contrast, members maintain a relatively consistent average ride length below 1000 seconds throughout the week.

Take a look at the total rides and average ride time for members and casual riders, categorized by each month.

```r
clean_tripdata$month <- ordered(clean_tripdata$month, 
                            levels=c("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"))

clean_tripdata %>% 
  group_by(member_casual, month) %>%  
  summarise(number_of_rides = n(), average_ride_length = mean(ride_length), .groups="drop") %>% 
  arrange(member_casual, month)
```
`Result:`

<div>
  <img width="602" alt="year_summary" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/fb0cfd80-f8be-43ae-87c4-d2fb4d4425fd">
</div>
<br>

**Visualize total rides data by type and month**

```r
clean_tripdata %>%  
  group_by(member_casual, month) %>% 
  summarise(number_of_rides = n(),.groups="drop") %>% 
  arrange(member_casual, month)  %>% 
  ggplot(aes(x = month, y = number_of_rides, fill = member_casual)) +
  labs(title ="Total Rides: Members vs. Casual Riders by Month", x = "Month", y= "Number Of Rides") +
  geom_col(position = "dodge") +
  theme(axis.text.x = element_text(angle = 45)) +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

`Result:`

<div>
  <img width="471" alt="monthly_total_rides " src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/71fd100a-ec81-4245-a765-ddc237ec981a">
</div>
<br> 

June, July, August, and September are the busiest months for both members and casual riders. This could be attributed to a decrease in total rides during the winter months of November, December, January, and February for both customer types. However, throughout the year, members generally have higher total rides compared to casual riders, except for June, July, and August.

**Visualize average ride time data by type and month**
```r
clean_tripdata %>%  
  group_by(member_casual, month) %>% 
  summarise(average_ride_length = mean(ride_length),.groups="drop") %>%
  ggplot(aes(x = month, y = average_ride_length, fill = member_casual)) +
  geom_col(position = "dodge") + 
  labs(title ="Average Ride Time: Members vs. Casual Riders by Month") +
  theme(axis.text.x = element_text(angle = 30))
  ```
  
`Result:`

<div>
  <img width="472" alt="Average_ride_time_month" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/1173353a-abfa-4126-bfe0-f6c24e1f9319">
</div>
<br> 

The average ride length for members remains consistently below 1000 seconds throughout the year. On the other hand, casual riders have an average ride length ranging between 1000 and 2000 seconds throughout the year. 

**Analyzing the bike demand at different hours of the day**

```r
clean_tripdata %>%
  ggplot(aes(start_time, fill= member_casual)) +
  labs(x="Time of day", title="Hourly bike demand", y = "Count") +
  geom_bar()+
  theme(plot.title = element_text(hjust = 0.5))
```

`Result:`

<div>
  <img width="443" alt="hourly_bike_demand" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/81b93b8f-12a0-45a4-99d5-39579cd671d9">
</div>
<br>

The graph displays a gentle rise in bike demand starting at 7am, reaching a peak in the afternoon around 7pm. This pattern suggests an increase in bike usage after regular working hours.

**Analyzing the bike demand per hour and categorized by the day of the week.**

```r
clean_tripdata %>%
    ggplot(aes(start_time, fill=member_casual)) +
    geom_bar() +
    labs(x="Time of day (hours).", title="Bike demand by hour and day of the week.") +
    theme(plot.title = element_text(hjust = 0.5))+
    facet_wrap(~ day_of_week)
 ```
 
 `Result:`

<div>
  <img width="470" alt="Hour_Weekday" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/3e8e5f37-b9b4-413e-bc69-65b8aac8fde5">
</div>
<br>
 
Weekdays and weekends have different bike usage patterns. On weekdays, there is a noticeable increase in bike volume during morning (7am-10am) and evening (5pm-7pm) hours, indicating members use bikes for daily commuting. On the other hand, weekends show a significant peak in bike volume for casual riders on Friday, Saturday, and Sunday. This suggests that casual riders use bikes more for leisure activities on weekends.

## Share

Now that you've finished analyzing the data and gained valuable insights, it's time to create eye-catching and polished visualizations to showcase your findings. It's essential to make sure these visualizations are sophisticated and well-presented, as they will help effectively communicate with the executive team. 

**<ins>Key Tasks</ins>**

* [x] Determine the best way to share your findings.
* [x] Create effective data visualizations.
* [x] Present your findings.
* [x] Ensure your work is accessible.

**<ins>Deliverable</ins>**

[x] Supporting visualizations and key findings

Key Insights and Concluding Findings

- Members account for the majority of total rides, approximately 18% more than casual riders, but casual riders account more for the average ride time.
- Throughout all months, the number of members exceeds that of casual riders.
- Casual riders contribute a significant volume of rides during the weekends.
- Afternoon hours exhibit a higher volume for riders.
- It is likely that members utilize bikes for work-related purposes, supported by the reduced amount of rides on weekends, the peeks before and after work, and the lower presence of casual riders during colder months.

Differences between Members and Casual Riders:

- Members generally contribute a larger volume of data, except on Saturdays and Sundays, where casual riders have the most data points.
- Casual riders tend to have longer ride durations compared to members. Members' average ride times remain relatively consistent, with a slight increase towards the end of the week.
- During the morning hours, particularly between 7am and 10am, there is a higher presence of members. In contrast, casual riders are more active between 3pm and 12am.
- Classic bikes are preferred by members, followed by electric bikes.
- Members demonstrate a more predictable usage pattern for routine activities, while casual riders exhibit different usage patterns, with the majority of their rides occurring on weekends.
- Casual riders tend to spend more time near the city center or the bay area, whereas members are dispersed throughout the city.

These insights can help the company identify areas for improvement and potential opportunities for attracting more riders.

## Act
Based on the insights gained from the analysis, the Cyclistic's executive team, alongside their Director of Marketing, Lily Moreno, and the Marketing Analytics team, can get down to work on the next steps the bike-share company might take.

**<ins>Deliverable</ins>**

[x] Your top three recommendations based on your analysis

1. Offer a weekend-only annual membership, with a different price point than the full annual membership. 
2. Optimize bike maintenance schedules to ensure maximum bike availability during peak hours.
3. Consider offering coupons and discounts for annual subscriptions or weekend-only memberships, specifically targeting casual riders.
4. Investigate further into the factors influencing ridership trends, such as weather conditions, local events, or e-bike usage.

By implementing these recommendations, the company can improve its services and attract more riders, ultimately leading to increased revenue and growth.

