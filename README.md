# Storm Events Data Analysis

## Overview
This project analyzes storm events data from NOAA's Storm Events Database for the year 1988. The data includes information about weather-related storm events such as start and end dates, locations, associated deaths, injuries, and property damage.

## Data
The data used in this project is obtained from the NOAA's Storm Events Database. The file used is `StormEvents_details-ftp_v1.0_d1988_c20220425.csv`.

## Steps Performed
1. Install and Load Necessary Packages:
    - `ggplot2`
    - `dplyr`
    - `stringr`
    - `tidyverse`

2. Read and Process Data:
    - Read the CSV file.
    - Select specific columns needed for analysis.
    - Arrange data by state name.
    - Change state and county names to title case.
    - Filter for events listed by county FIPS.
    - Pad state and county FIPS with zeros and unite them into a single FIPS column.
    - Change all column names to lower case.

3. Create Additional Dataframe:
    - Create a dataframe with state name, area, and region information.
    - Count the number of events per state.
    - Merge the events data with state information.

4. Create Plot:
    - Generate a scatter plot showing the number of storm events vs. land area of each state, colored by region.

## How to Run the Script
1. **Ensure all necessary packages are installed**:
    ```r
    install.packages("ggplot2")
    install.packages("dplyr")
    install.packages("stringr")
    install.packages("tidyverse")
    ```

2. **Load the packages**:
    ```r
    library(ggplot2)
    library(dplyr)
    library(stringr)
    library(tidyverse)
    ```

3. Run the script:
    - Place the `StormEvents_details-ftp_v1.0_d1988_c20220425.csv` file in the same directory as your script or specify the correct file path.
    - Run the script `Assignment 3.R` to process the data and generate the plot.

## Example
Here is a brief example of the code used in the script:

```r
# Load necessary libraries
library(dplyr)
library(stringr)
library(tidyverse)
library(ggplot2)

# Read the CSV file into a dataframe
file_path <- "StormEvents_details-ftp_v1.0_d1988_c20220425.csv"
storm_data <- read.csv(file_path)

# Data processing steps...

# Create the plot
storm_plot <- ggplot(merged_data, aes(x = area, y = number_of_events)) +
  geom_point(aes(color = region)) +
  labs(x = "Land area (square miles)",
       y = "# of storm events in 1988")

# Print the plot
print(storm_plot)
