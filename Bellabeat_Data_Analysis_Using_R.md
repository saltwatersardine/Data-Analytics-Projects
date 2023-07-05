## Introduction

Bellabeat is a high-tech manufacturer of health-focused products for women. I will analyze smart device data to gain insights into how consumers use their smart devices. My analysis will help guide future marketing strategies for the team. 

In order to answer the key business questions, I will follow the steps of the data analysis process: `ask`, `prepare`, `process`, `analyze`, `share`, and `act`.

Along the way, the Case Study Roadmap will be used as a guide for each data analysis process: `code`: when needed, `key tasks`: as a checklist, and `deliverable`: as a checklist.

For the analysis, I will be using the `R` programming language and `RStudio` for its easy analysis tools and data visualizations.

## Scenario

You are a junior data analyst working on the marketing analyst team at Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, co-founder and Chief Creative Officer of Bellabeat believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. You have been asked to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. The insights you discover will then help guide the marketing strategy for the company. You will present your analysis to the Bellabeat executive team along with your high-level recommendations for Bellabeat’s marketing strategy. 

The next step is to `Ask` questions that clarify the problem, making sure we completely understand stakeholder expectations.

## Ask

To examine Bellabeat's available consumer data and identify growth opportunities, we must understand the problem at hand, which involves analyzing smart device usage data for a specific Bellabeat product. This analysis aims to identify trends in smart device usage, determine their relevance to Bellabeat's customers, explore their potential impact on Bellabeat's marketing strategy, and guide the process with three key questions:

  - What features are most used? 
  - How long do users engage with the product daily? 
  - What are the peak usage times?

**Guiding Questions:**

1. What is the problem you are trying to solve?
   - The problem we are trying to solve is to understand the trends in the usage of non-Bellabeat smart devices by consumers. We aim to apply these insights to one of Bellabeat's products to guide its marketing strategy.
2. How can the insights drive business decisions?
   - The insights obtained from the analysis can help inform the marketing strategy for Bellabeat. By understanding how consumers use smart devices, we can identify potential areas for promoting Bellabeat products, enhancing existing features, or developing new features that meet the needs and preferences of current and potential customers.

**<ins>Key Tasks:</ins>**
* [x] Identify the business task
  - The business task is to analyze smart device usage data to identify trends and apply these insights to one of Bellabeat's products to guide its marketing strategy. 
* [x] Key stakeholders
  - **Urška Sršen** (Bellabeat’s co-founder and Chief Creative Officer), 
  - **Sando Mur** (co-founder and key member of the Bellabeat executive team), and 
  - **Bellabeat marketing analytics team**.

**<ins>Deliverable:</ins>**
* [x] A clear statement of the business task:
  - The objective of this project is to analyze consumer usage data from non-Bellabeat smart devices. The insights gained from this analysis will then be applied to one selected Bellabeat product, with the aim of guiding the company's marketing strategy. The focus will be on identifying patterns and trends in smart device usage, determining their relevance to Bellabeat customers, and understanding how these trends could potentially influence Bellabeat’s marketing strategy.

Next, we proceed with the `Prepare` phase of the project.

## Prepare

Let's proceed with the 'Prepare' stage of the project making sure the dataset is clean and well-structured before proceeding with the analysis.

**Guiding Questions:**

1. How does it help you answer your question?
   - This dataset contains detailed information about the users' activity, steps, and heart rate - key factors that can help us understand user habits around smart devices. This information is crucial to answering our key questions.
2. Are there any problems with the data?
   - We'll need to download and thoroughly analyze the dataset to identify any potential issues.

**<ins>Key Tasks:</ins>**
* [x] Download data and store it appropriately.
  - The data we are using is stored and saved in a folder created on my desktop using appropriate file-naming conventions. It will also be stored on Github.
* [x] Identify how it’s organized.
  - We'll need to download and explore the dataset to determine its organization and format.
* [x] Sort and filter the data.
  - There were 18 .csv files in total, but I am choosing to focus on these 6: *dailyActivity_merged.csv*, *dailyCalories_merged.csv*, *dailyIntensities_merged.csv*, *dailySteps_merged.csv*, *sleepDay_merged.csv*, *weightLogInfo_merged.csv*.
* [x] Determine the credibility of the data.
  - The dataset is labeled as 'CC0: Public Domain,' meaning we can use it freely. It also consists of data from users who have consented to the submission of their personal data. However, we need to ensure that the data remains anonymous and we respect users' privacy.

**<ins>Deliverable:</ins>**
* [x] A description of all data sources used:
  - The primary data source for this analysis is the [Fitbit Fitness Tracker Data](https://www.kaggle.com/arashnic/fitbit), available on Kaggle.

### Step 1: Load Packages

Start by installing the required packages: `tidyverse`, `lubridate`, `janitor`, `ggplot2`, `dplr` and `readr`

```r
install.packages(c("tidyverse", "lubridate", "janitor", "ggplot2", "dplyr", "readr"))
```

Once a package is installed, you can load it by running the library() function

```r
library(tidyverse)
library(lubridate)
library(janitor)
library(ggplot2)
library(dplyr)
library(readr)
```

### Step 2: Import and Merge Data

Method 1: Each file is read using the read_csv(), and the resulting data frames are individually added. 

```r
daily_activity <- read_csv("bellabeat_data/dailyActivity_merged.csv")
daily_calories <- read_csv("bellabeat_data/dailyCalories_merged.csv")
daily_intensities <- read_csv("bellabeat_data/dailyIntensities_merged.csv")
daily_steps <- read_csv("bellabeat_data/dailySteps_merged.csv")
daily_sleep <- read_csv("bellabeat_data/sleepDay_merged.csv")
weight_info <- read_csv("bellabeat_data/weightLogInfo_merged.csv")
```
  
We are now ready to move on to the `Process` phase of the project.

## Process

**<ins>Key Tasks:</ins>**
* [x] Check the data for errors.
  - Get to know your data by using the `head()`, `str()`, `colnames()`, and `summary()` functions to view and summarize data.
* [x] Choose your tools.
  - R and RStudio as my main tool for data cleaning and analysis.
* [x] Transform the data so you can work with it effectively.
* [x] Document the cleaning process.
  - I will document each step of the cleaning process in a reproducible R script. The cleaning process would involve steps like removing duplicates, handling missing or inconsistent data, and converting data types if needed.

**<ins>Deliverable:</ins>**
* [x] Documentation of any cleaning or manipulation of data:
  - Add underscores between the words in column names and changed all column names to lowercase.
  - Rename date columns and correct date formatting.  
  - Add a date column to all the datasets in date format for merging later.
  - Removed columns that were no longer required for the analysis process.

### Step 1: Inspect the Data:
Start by checking for missing or inconsistent data. Look for outliers or unusual values, and check the data types of each column to make sure they match what you would expect (e.g., numerical columns should be stored as numbers, dates as date types, etc.)

Checking for Distinct Entries
```r
daily_activity %>% summarise(n_distinct(Id))
daily_calories %>% summarise(n_distinct(Id))
daily_intensities %>% summarise(n_distinct(Id))
daily_sleep %>% summarise(n_distinct(Id))
daily_steps %>% summarise(n_distinct(Id))
weight_info %>% summarise(n_distinct(Id))
```

There are some issues with the timestamp data. Before analyzing, it needs to be converted into a date-time format and split into separate date and time components.
  
### Step 2: Transform the Data
This might involve creating new columns, aggregating data, reshaping the data, or other transformations. For example, the sleep and weight datasets include date and time.

Let's tidy up and standardize the column names by adding an underscore.

```r
daily_activity <- clean_names(daily_activity)
daily_calories <- clean_names(daily_calories)
daily_intensities <- clean_names(daily_intensities)
daily_sleep <- clean_names(daily_sleep)
daily_steps <- clean_names(daily_steps)
weight_info <- clean_names(weight_info)
```
Let's go ahead and lowercase those column names.

```r
daily_activity=rename_with(daily_activity,tolower)
daily_calories=rename_with(daily_calories,tolower)
daily_intensities=rename_with(daily_intensities,tolower)
daily_sleep=rename_with(daily_sleep,tolower)
daily_steps=rename_with(daily_steps,tolower)
weight_info=rename_with(weight_info,tolower)
```
We'll use the rename() function to give them better names, and then the mutate() function to convert them into the correct data types.

```r
daily_activity <- daily_activity %>% 
  rename(date = activity_date) %>%
  mutate(date = as.Date(date, format = "%m/%d/%y"))
  
daily_calories <- daily_calories %>% 
  rename(date = activity_day) %>%
  mutate(date = as.Date(date, format = "%m/%d/%y"))

daily_intensities <- daily_intensities %>% 
  rename(date = activity_day) %>%
  mutate(date = as.Date(date, format = "%m/%d/%y"))

daily_steps <- daily_steps %>% 
  rename(date = activity_day) %>%
  mutate(date = as.Date(date, format = "%m/%d/%y"))
  
daily_sleep <- daily_sleep %>% 
  rename(date = sleep_day) %>% 
  mutate(date = as.Date(date, format = "%m/%d/%y"))

weight_info <- weight_info %>% 
  select(-log_id) %>% 
  mutate(date = as.Date(date, format = "%m/%d/%y")) %>% 
  mutate(is_manual_report = as.factor(is_manual_report)) # as.factor converts a variable into a categorical factor.

```

### Step 3: Merge the Data 

```r
merged_data <- merge(merge(daily_activity, daily_sleep, by=c('id','date'), all = TRUE), 
                     weight_info, by = c('id','date'), all = TRUE)
```

### Step 4: Remove extra Variables 

```r
merged_data <- merged_data %>% 
  select(-c(tracker_distance, logged_activities_distance, total_sleep_records, weight_pounds, fat, bmi, is_manual_report))
```
### Step 5: Adding new Columns

We need to create new columns to aggregate the data by date (e.g. daily, weekly, monthly, yearly). 

```r
final_data <- transform (merged_data, 
         year = format(as.Date(date), "%Y"),          # Year (4 digit).
         month = format(as.Date(date), "%B"),         # Full month.
         day = format(as.Date(date), "%d"),           # Decimal date.
         weekday = format(as.Date(date), "%A"))       # Full weekday.
```
We dont use the weekdays() function here because we want to keep data. 

### Step 6: Putting Weekdays in Order

```r
final_data$weekday <- ordered(final_data$weekday, 
                                      levels=c("Sunday","Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```

With the dates appropriately formatted and all data frames thoroughly reviewed for errors, I can now proceed to the `Analyze` phase of the project.

## Analyze

**<ins>Key Tasks:</ins>**
* [x] Aggregate your data so it’s useful and accessible.
* [x] Organize and format your data.
* [x] Perform calculations.
* [x] Identify trends and relationships.

**<ins>Deliverable:</ins>**
* [x] A summary of your analysis:

### Step 1: Find the Mean, Median, Max, and Min

Mean, Median, Max, and Min for total steps. 

```r
final_data %>%
  summarise(average_steps = mean(total_steps), median_length = median(total_steps), 
            max_steps = max(total_steps), min_steps = min(total_steps))
```

`Result:`
<div>
  <img width="1000" alt="AverageSteps" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/122704b5-5495-4a25-8203-60a4e6efb062">
</div>
<br>
<br>

Let's check if there's a link between the total steps and the day of the week.

```r
ggplot(data = final_data) +
  geom_bar(mapping = aes(x = weekday, y = total_steps, fill = weekday), stat = "sum") +
  labs(title = "TOTAL STEPS BY WEEKDAY", x = "Weekday", y = "Total Steos") +
  theme(plot.title = element_text(hjust = 0.5), legend.position = "none") +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

`Result:`
<div>
  <img width="530" alt="TotalSteps_DayOfWeek" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/21c13183-b3a6-44e8-bb0b-637049540212">
</div>
<br>
<br>

Mean, Median, Max, and Min for total calories. 

```r
final_data %>%
  summarise(average_calories = mean(calories), median_length = median(calories), 
            max_steps = max(calories), min_steps = min(calories))
```

`Result:`
<div>
<img width="1000" alt="averageCalories" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/72e36688-55f6-404a-8e7f-906c62a5657d">
</div>
<br>
<br>

Let's check if there's a link between the calories and the day of the week.

```r
ggplot(data = final_data) +
  geom_bar(mapping = aes(x = weekday, y = calories, fill = weekday), stat = "sum") +
  labs(title = "CALORIES BY WEEKDAY", x = "Weekday", y = "Total Calories") +
  theme(plot.title = element_text(hjust = 0.5), legend.position = "none") +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```

`Result:`
<div>
  <img width="530" alt="TotalCalories_DayOfWeek" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/6ab1845e-a0e5-40cc-bbad-5f1863cef56c">
</div>
<br>

Let's check if there's a link between the total steps and and calories.

```r
ggplot(data = final_data) +
  geom_point(mapping = aes(x = total_steps, y = calories)) +
  geom_smooth(mapping = aes(x = total_steps, y = calories), colour = "blue") +
  labs(title = "TOTAL STEPS VS CALORIES", x = "Total Steps", y = "Total Calories") +
  theme(plot.title = element_text(hjust = 0.5), legend.position = "none") +
  scale_y_continuous(labels = function(x) format(x, scientific = FALSE))
```
`Result:`
<div>
  <img width="530" alt="totalsteps_totalcals" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/ab800b1e-26a1-4dcb-8a1c-218f43a7cf28">
</div>
<br>

These two variables show a strong positive correlation: as the number of steps taken in a day increases, the amount of calories burned also increases.

Now lets see the differences in differences for each activity.

```r
ggplot(data = gather(summarise(final_data,
                               "Sedentary Distance" = sum(sedentary_active_distance, na.rm = TRUE),
                               "Very Active Distance"= sum(very_active_distance, na.rm = TRUE),
                               "Moderately Active Distance" = sum(moderately_active_distance, na.rm = TRUE),
                               "Light Distance" = sum(light_active_distance, na.rm = TRUE)), 
                     Activity, Distance)) +
  geom_bar(aes(x = Activity, y = Distance, fill = Activity), 
           stat = "identity", position = "dodge") +
  labs(x = "Activity", y = "Total Distance", title = "Total Distance per Activity Level") +
  theme(plot.title = element_text(hjust = 0.5), legend.position = "none")
```

`Result:`
<div>
  <img width="428" alt="Distance_activity" src="https://github.com/saltwatersardine/Data-Analytics-Projects/assets/109593672/86d17982-f08e-4b96-a2e6-38956f6ac33f">
</div>
<br>




Now, let's proceed to the next phase of the project, which is the `Share` phase. 
