# ANA-515_Week-6
Assignment 3
getwd()
library(dplyr)
library(stringr)
library(tidyverse)

# Read the CSV file into a dataframe
storm_data <- read.csv("/Users/odinakaezeobele/Desktop/StormEvents_details-ftp_v1.0_d1988_c20220425.csv")

# Check the first few rows to ensure the data is read correctly
head(storm_data)

# Limit the dataframe to the specified columns
columns_to_keep <- c("BEGIN_YEARMONTH", "EPISODE_ID", "STATE", "STATE_FIPS", "CZ_NAME", "CZ_TYPE", "CZ_FIPS", "EVENT_TYPE")
storm_data <- storm_data %>% select(all_of(columns_to_keep))

# Arrange the data by the state name
storm_data <- storm_data %>% arrange(STATE)

# Change state and county names to title case
storm_data <- storm_data %>% mutate(STATE = str_to_title(STATE), CZ_NAME = str_to_title(CZ_NAME))

# Limit to the events listed by county FIPS and remove the CZ_TYPE column
storm_data <- storm_data %>% filter(CZ_TYPE == "C") %>% select(-CZ_TYPE)

# Pad the state and county FIPS with a “0” and unite the two columns
storm_data <- storm_data %>% 
  mutate(STATE_FIPS = str_pad(STATE_FIPS, 3, pad = "0"),
         CZ_FIPS = str_pad(CZ_FIPS, 3, pad = "0")) %>%
  unite("FIPS", STATE_FIPS, CZ_FIPS, sep = "")

# Change all the column names to lower case
storm_data <- storm_data %>% rename_all(tolower)


# Create a dataframe with state name, area, and region
data("state")
state_info <- data.frame(state = tolower(state.name), 
                         area = state.area, 
                         region = state.region)

# Create a dataframe with the number of events per state
events_per_state <- storm_data %>% group_by(state) %>% summarize(number_of_events = n())

# Make state names lowercase to match the state_info dataframe
events_per_state <- events_per_state %>% mutate(state = tolower(state))

# Merge with state_info
merged_data <- events_per_state %>% inner_join(state_info, by = c("state" = "state"))

# Check the structure of the merged data
str(merged_data)

install.packages("ggplot2")
library(ggplot2)

# Create the plot
storm_plot <- ggplot(merged_data, aes(x = area, y = number_of_events)) +
  geom_point(aes(color = region)) +
  labs(x = "Land area (square miles)",
       y = "# of storm events in 1988")

# Print the plot
print(storm_plot)
