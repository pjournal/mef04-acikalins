
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)

library(tidyverse)
library(lubridate)
library(dplyr)

df_epias <- read_csv("/Users/galdor/Desktop/PYTHON 3/MEF/Fall/BDA503 - Data Analytics Essentials/Assignments/Assignment 2/EPIAS_DATA_ptf-smf.csv")

```

```{r df_epias, message = FALSE}


df_epias_MCP_SMP_hour <- df_epias %>%
  select(Date, MCP, SMP)

df_epias_daily_mean_MCP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2), operator="Mean_MCP", operator2='Mean') %>%
  select(Date_day, MCP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=mean(MCP))

df_epias_daily_mean_SMP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Mean_SMP", operator2='Mean') %>%
  select(Date_day, SMP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=mean(SMP))

df_epias_daily_median_MCP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Median_MCP", operator2='Median') %>%
  select(Date_day, MCP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=median(MCP))

df_epias_daily_median_SMP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Median_SMP", operator2='Median') %>%
  select(Date_day, SMP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=median(SMP))


df_epias_daily_max_MCP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Max_MCP", operator2='Max') %>%
  select(Date_day, MCP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=max(MCP))

df_epias_daily_max_SMP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Max_SMP", operator2='Max') %>%
  select(Date_day, SMP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=max(SMP))

df_epias_daily_min_MCP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Min_MCP", operator2='Min') %>%
  select(Date_day, MCP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=min(MCP))

df_epias_daily_min_SMP <- df_epias %>%
  mutate(Date_day=substr(Date, 1, 2),  operator="Min_SMP", operator2='Min') %>%
  select(Date_day, SMP, operator, operator2) %>%
  group_by(Date_day, operator, operator2) %>%
  summarize(value_day=min(SMP))

df_epias_daily_merge_1 <- merge(x=df_epias_daily_mean_MCP, y=df_epias_daily_mean_SMP, all=TRUE)
df_epias_daily_merge_2 <- merge(x=df_epias_daily_median_MCP, y=df_epias_daily_median_SMP, all=TRUE)
df_epias_daily_merge_3 <- merge(x=df_epias_daily_max_MCP, y=df_epias_daily_max_SMP, all=TRUE)
df_epias_daily_merge_4 <- merge(x=df_epias_daily_min_MCP, y=df_epias_daily_min_SMP, all=TRUE)
df_epias_daily_merge_sub_1 <- merge(x=df_epias_daily_merge_1, y=df_epias_daily_merge_2, all=TRUE)
df_epias_daily_merge_sub_2 <- merge(x=df_epias_daily_merge_3, y=df_epias_daily_merge_4, all=TRUE)
df_epias_daily_merge <- merge(x=df_epias_daily_merge_sub_1, y=df_epias_daily_merge_sub_2, all=TRUE)

```

# A Short Review of EPIAS September'20 Electricty Market Prices

### A Glance at the Data

Dataframe consists of hourly realized electricity prices, imbalance prices and SMP Direction values
  - MCP : Market Clearing Price
  - SMP : System Marginal Price

``` {r }
glimpse(df_epias)

```

For other details of how the market works link is provided:
[Mini Tutorial about Electricity Market Prices](https://pjournal.github.io/files/electricity_markets_mini_tutorial)

### Summary of Hourly MCP and SMP Over 720 hours of Sep'20 


```{r }

summary(df_epias_MCP_SMP_hour %>% select(MCP,SMP))

```


Average hourly MCP and average hourly SMP is seemed not to met with each other most of the time. This gives a clue about most of the days and hours in September'20 electricity markets closed with an imbalanced price.





### Daily Average and Median Values of MCP and SMP

```{r }

ggplot(df_epias_daily_merge_sub_1, aes(x=Date_day, y=value_day, color=operator)) + geom_point() +  expand_limits(y=0) + facet_wrap(~ operator2) + ggtitle("Comparison over daily average and median values of Sep'20") + guides(x = guide_axis(angle = 90))

```

It is easier to tell which day is mostly ended with energy surplus or energy deficit by looking at daily averages or medians. Looking at the graphs:

  - On 3rd, 5th, 17th and 19th of Sep'20 you may see obvious extreme SMP values caused deficit or surplus.
  - Median of MCP values could lead the interpretation of general daily demand forecast is centered between 300 and 325.
  - Between 20th and 27th of Sep'20 a slight increase in the demand for electricity is seemed to be happened with a negative imbalance.


### Daily Min and Max Values of MCP and SMP

```{r warning  =FALSE}

ggplot(df_epias_daily_merge_sub_2, aes(x=Date_day, y=value_day, color=operator)) + geom_point() +  expand_limits(y=0) + facet_wrap(~ operator2) + ggtitle("Comparison over daily max and min values of Sep'20") + guides(x = guide_axis(angle = 90)) + scale_y_log10()

```

When we look at the min and max graphs:

  - On 3rd, 7th and 17th of Sep'20 instant peaks in electricty demands are observed since SMP realization is way too high. 
  - Contrarily on 7th and 19th of Sep'20 the two lowest hourly SMP realization is seen over the series.
  - These extreme values could lead interpretations such as on these hours extraneous events may occured that maximizes or minimizes the energy consumption or causes problems on the energy production side.
