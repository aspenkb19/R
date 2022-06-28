# Semmel-Handwashing-Project

## Overview

#### How Semmelweis, working in an obstetrics ward in Vienna in the 19th century, made the connection between dirty hands and deadly infection.

Start by looking at the data that made Semmelweis realize that something 
 was wrong with the procedures at Vienna General Hospital. We have yearly data for 
 two clinics to analyze. 
 
```library(tidyverse)
yearly <- read_csv("yearly_deaths_by_clinic.csv")
```

## Visualize the Data

Add a new column to yearly with proportion of deaths per no. births.

```yearly <- yearly %>%
  mutate(proportion_deaths = deaths/births)
```

Plot yearly proportion of deaths at the two clinics.

```options(repr.plot.width=7, repr.plot.height=4)
ggplot(yearly, aes(x = year, y = proportion_deaths, color =clinic)) +geom_line()
```

## Add New Information

After handwashing was implemented, monthly data was collected. 

```monthly <- read_csv("monthly_deaths.csv")
```
Adding a new column with proportion of deaths per no. births.

```monthly <- monthly %>% 
  mutate(proportion_deaths = deaths/births)
```

## New Visualization

Plot monthly proportion of deaths.

```ggplot(monthly, aes(x = date, y = proportion_deaths)) +geom_line()
```

# Determine Effect of Intervention

From this date handwashing was made mandatory: ('1847-06-01')

```handwashing_start = as.Date('1847-06-01').
```

Add a TRUE/FALSE column to monthly called handwashing_started.

```monthly <- monthly %>%
  mutate(handwashing_started = date >= handwashing_start)
```

Plot monthly proportion of deaths before and after handwashing.

```ggplot(monthly, aes(x = date, y = proportion_deaths, color = handwashing_started)) +geom_line()
```


# Calculating the mean proportion of deaths before and after handwashing.

```monthly_summary <- monthly %>% 
  group_by(handwashing_started) %>%
  summarise(mean_proportion_deaths = mean(proportion_deaths))
```

Calculate a 95% Confidence intrerval using t.test.

```test_result <- t.test(proportion_deaths ~ handwashing_started, data = monthly)
```

## Conclusion

Doctors not washing their hands increased the proportion of deaths by between 6.7 and 10 percentage points, according 
to a 95% confidence interval. All in all, it would seem that Semmelweis had solid evidence that handwashing was a simple
but highly effective procedure that could save many lives.
