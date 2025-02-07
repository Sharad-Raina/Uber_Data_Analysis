# Uber Data Analysis Project

This project analyzes Uber ride data from April to September 2014 to uncover patterns in ride demand based on time, day, and month. The insights gained can help optimize resource allocation, pricing strategies, and operational efficiency.

<img width="607" alt="Screenshot 2025-02-07 at 6 40 54 PM" src="https://github.com/user-attachments/assets/1f771b8a-75fa-4111-bd25-9fe47f9ab2dc" />


## Dataset
The dataset consists of Uber ride records from six months (April–September 2014) and includes:
- `Date/Time`: Timestamp of the ride
- `Latitude` and `Longitude`: Geographical location
- `Base`: Base code assigned to Uber rides

## Technologies Used
- **R** (Tidyverse, Lubridate, ggplot2)
- **Data Manipulation & Visualization**
- **Time Series Analysis**

## Data Preparation & Cleaning

```r
# Loading necessary libraries
library(tidyverse)
library(lubridate)

# Reading CSV files
april_data <- read_csv("uber-raw-data-apr14.csv", show_col_types = FALSE)
may_data <- read_csv("uber-raw-data-may14.csv", show_col_types = FALSE)
# Repeat for other months

data_2014 <- rbind(april_data, may_data) # Append all dataframes

# Convert Date/Time column to POSIXct
data_2014$`Date/Time` <- as.POSIXct(data_2014$`Date/Time`, format = "%m/%d/%Y %H:%M:%S")

# Extract time-related features
data_2014$Hour <- hour(data_2014$`Date/Time`)
data_2014$Weekday <- wday(data_2014$`Date/Time`, label = TRUE)
data_2014$Monthname <- month(data_2014$`Date/Time`, label = TRUE)

data_2014 <- drop_na(data_2014) # Remove missing values
```

## Exploratory Data Analysis

### Trips by Hour
```r
data_2014 %>%
  group_by(Hour) %>%
  summarise(Total = n()) %>%
  ggplot(aes(factor(Hour), Total / 1000)) +
  geom_bar(stat = "identity", fill = "red") +
  labs(title = "Total Trips by Hour", x = "Hour of the Day", y = "Total Trips (in K)") +
  theme_classic()
```
**Insights:**
- Lowest trips occur between 2-4 AM.
- Peak hours are between 5-9 PM, aligning with evening rush.

### Trips by Month
```r
data_2014 %>%
  group_by(Monthname) %>%
  summarise(Total = n()) %>%
  ggplot(aes(Monthname, Total / 1000)) +
  geom_bar(stat = "identity", fill = "red") +
  labs(title = "Trips for Each Month", x = "Month", y = "Total Trips (in K)") +
  theme_classic()
```
**Insights:**
- August & September have the highest demand.
- Possible factors: vacations, events, weather conditions.

### Trips by Day of the Week
```r
data_2014 %>%
  group_by(Monthname, Weekday) %>%
  summarise(Total = n()) %>%
  ggplot(aes(Monthname, Total / 1000, fill = Weekday)) +
  geom_bar(stat = "identity", position = "dodge") +
  labs(title = "Trips by Month and Weekday", x = "Month", y = "Total Trips (in K)") +
  theme_classic()
```
**Insights:**
- Thursdays, Fridays, and Saturdays see the highest demand.
- Weekends and evenings are prime times for Uber services.

## Business Implications
- **Driver Allocation**: Increase driver availability during peak hours and days.
- **Pricing Strategies**: Implement surge pricing in high-demand periods.
- **Marketing Campaigns**: Target high-traffic months with promotions.

## Conclusion
This analysis provides valuable insights into Uber ride demand trends. By leveraging time-based patterns, Uber can improve service efficiency, customer satisfaction, and profitability.

