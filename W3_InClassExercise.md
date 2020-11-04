
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = FALSE)

library(tidyverse)
library(lubridate)
library(dplyr)
library(readxl)

```

```{r message = FALSE}

df_konut <- read_excel("/Users/galdor/Desktop/PYTHON 3/MEF/Fall/BDA503 - Data Analytics Essentials/In Class Exercises/EVDS_istanbul_property_data.xlsx", n_max=130) %>%
  rename(Date_1=1, Total=2, Mortgage=3, New_Sales=4, Second_Hand=5, To_Foreign=6, New_House_P_Index=7, Ist_Level=8, House_Unit_P=9)

df_konut_satis_pivot <- df_konut %>%
  pivot_longer(!Date_1, names_to="Sales_Type", values_to="Amount") %>%
  filter(Sales_Type %in% c('Total','Mortgage','New_Sales', 'Second_Hand'), Date_1 >= 2013)

df_konut_satis_pivot_new_vs_sec <- df_konut %>%
  pivot_longer(!Date_1, names_to="Sales_Type", values_to="Amount") %>%
  filter(Sales_Type %in% c('New_Sales', 'Second_Hand'), Date_1 >= 2013)

df_konut_satis_pivot_total_vs_mortgage <- df_konut %>%
  pivot_longer(!Date_1, names_to="Sales_Type", values_to="Amount") %>%
  filter(Sales_Type %in% c('Total','Mortgage'), Date_1 >= 2013)

df_konut_satis_pivot_total_vs_unit <- df_konut %>%
  pivot_longer(!Date_1, names_to="Sales_Type", values_to="Amount") %>%
  filter(Sales_Type %in% c('Total','House_Unit_P'), Date_1 >= 2013)

```


## Quick Look on Dataframe
```{r }
glimpse(df_konut)

```

## New House Sales vs Second Hand Sales

```{r warning = FALSE}

ggplot(df_konut_satis_pivot_new_vs_sec, aes(x=Date_1, y=Amount, color=Sales_Type)) + geom_point() + guides(x = guide_axis(angle = 90)) + ggtitle("New House Sales and Second Hand Sales Volume Over Years")

```

It is possible to say that in recent months people's preference is shifted to second hand house sales.





## Total House Sales vs Mortgage Backed Sales

```{r warning = FALSE}

ggplot(df_konut_satis_pivot_total_vs_mortgage, aes(x=Date_1, y=Amount, color=Sales_Type)) + geom_point() + guides(x = guide_axis(angle = 90)) + ggtitle("Total House Sales and Mortgage Sales Volume Over Years")

```

It might look like after 2020-06 mortgage sales are sky rocketed till the end of 2020-08. Intuitively the reason behind this peak would be the decrease in mortgage credit rates.



## Total House Sales vs Unit Price

```{r warning = FALSE}

ggplot(df_konut_satis_pivot_total_vs_unit, aes(x=Date_1, y=Amount, color=Sales_Type)) + geom_point() + guides(x = guide_axis(angle = 90)) + ggtitle("Total House Sales vs Unit House Price Volume Over Years")

```

- From 2013 to 2017 although unit price is increased, total house sales do not show a stable increase at the same time interval. 
- After 2017 destabilization of the market could be seen with an obvious fluctutation of Total House Sales Volume.




