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
  - The data we are using is stored and saved in a folder created on my desktop using appropriate file-naming conventions it will also be stored on Github.
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

Method 1: Each file is read using the read_csv(), and the resulting dataframes are individually added. 

```r
activity_daily <- read_csv("bellabeat_data/dailyActivity_merged.csv")
calories_daily <- read_csv("bellabeat_data/dailyCalories_merged.csv")
intensities_daily <- read_csv("bellabeat_data/dailyIntensities_merged.csv")
steps_daily <- read_csv("bellabeat_data/dailySteps_merged.csv")
sleep_daily <- read_csv("bellabeat_data/sleepDay_merged.csv")
weight_daily <- read_csv("bellabeat_data/weightLogInfo_merged.csv")
```

Method 2: Each file is read using read_csv(), and the resulting dataframes are combined row-wise into a single dataframe.  

```r
all_fitnessdata <- 
  list.files(path = "<FILE PATH>", pattern = "*.csv", full.names = TRUE) %>%   
  map_df(read_csv)  
```

* list.files finds all the csv files in a folder. Replace <FILE PATH> with the path to your folder.
* full.names gives you the complete file path, not just the file name.
* map_df uses the full path to easily find and open each file.
  
We are now ready to move on to the `Process` phase of the project.

## Process

**<ins>Key Tasks:</ins>**
* [x] Check the data for errors.
  -   - Get to know your data by using the `head()`, `str()`, `colnames()`, and `summary()` functions to view and summarize data.
* [x] Choose your tools.
  - R and RStudio as my main tool for data cleaning and analysis.
* [x] Transform the data so you can work with it effectively.
* [x] Document the cleaning process.
  - I will document each step of the cleaning process in a reproducible R script. The cleaning process would involve steps like removing duplicates, handling missing or inconsistent data, and converting data types if needed.

**<ins>Deliverable:</ins>**
* [x] Documentation of any cleaning or manipulation of data:

### Step 1: Inspect the Data:
Start by checking for missing or inconsistent data. Look for outliers or unusual values, and check the data types of each column to make sure they match what you would expect (e.g., numerical columns should be stored as numbers, dates as date types, etc.)
  
There are some issues with the timestamp data. Before analyzing, it needs to be converted it into a date-time format and split it into separate date and time components.
  
### Step 2: Transform the data
This might involve creating new columns, aggregating data, reshaping the data, or other transformations. For example, the sleep and weight datasets include date and time.

Use the `separate()` function to split the `sleep_daily` and `weight_daily` date columns into their own date and time columns.

```r
sleep_daily <- sleep_daily %>% 
  separate(SleepDay,c("SleepDate","SleepTime")," ")
  
weight_daily <- weight_daily %>% 
  separate(Date,c("WeightDate","WeightTime")," ")
```

Or, if you're working with the combined data, you could separate the columns all at once.

```r
all_fitnessdata <- all_fitnessdata %>%
  separate(SleepDay, c("SleepDate", "SleepTime"), sep = " ") %>%
  separate(Date, c("WeightDate", "WeightTime"), sep = " ")
```

With the dates appropriately formatted and all dataframes thoroughly reviewed for errors, I can now proceed to the 'Analyze' phase of the project.
  
