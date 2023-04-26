# Data-Analytics-Projects

This repository contains a collection of my data analytics projects. Each project is contained within its own directory and includes the source code, data, and a README file with a description of the project and its results.

### Project List
1. [Cyclistic bike-share analysis](https://github.com/saltwatersardine/Data_Analytics/blob/main/cyclistic_bike_share_analysis.md)
2. [Bellabeat data analysis](https://github.com/saltwatersardine/Data_Analytics/blob/main/bellabeat_data_analysis.md)

### Software

Projects in this repository will be analysed using the following softwares:

- R
- SQL

You will also need to have the following R packages installed:

- `tidyverse:` Helps wrangle data
- `ggplot2:` Helps visualize data
- `lubridate:` Helps wrangle data attributes
- `readr:` Helps import data ('csv', 'tsv', and 'fwf')
- `data.table:` Helps with large datasets
- `dplyr:` Helps manipulate data


You can install these packages by running the following code in R:

```r
install.packages(c("tidyverse", "ggplot2", "lubridate", "readr", "data.table", "dplyr"))
```

Once packages are installed, you need load them by running the library function:

```r
library(tidyverse, ggplot2, lubridate, readr, data.table, dplyr)
```
### Usage

To run any of the projects, navigate to the project directory and run the R script:

```r
cd cyclistic_bikeshare_analysis
open cyclistic_bikeshare_analysis.R
```
Each project directory includes a README file that provides more detailed instructions on how to run the project and interpret the results.
