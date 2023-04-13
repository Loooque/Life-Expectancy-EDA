Group Project
================
Luke Andrade
2023-04-13

## Set-up

``` r
library(tidyverse)
```

    ## -- Attaching core tidyverse packages ------------------------ tidyverse 2.0.0 --
    ## v dplyr     1.1.1     v readr     2.1.4
    ## v forcats   1.0.0     v stringr   1.5.0
    ## v ggplot2   3.4.2     v tibble    3.2.1
    ## v lubridate 1.9.2     v tidyr     1.3.0
    ## v purrr     1.0.1     
    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()
    ## i Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(gapminder)
```

## Data Files

``` r
x1 <- read_csv("datasets/at_least_basic_water_source_overall_access_percent.csv") %>%
  select(country, `2009`) %>% rename('water_source_access' = `2009`)
x2 <- read_csv("datasets/child_mortality_0_5_year_olds_dying_per_1000_born.csv") %>%
  select(country, `2009`) %>% rename('child_mortality' = `2009`)
x3 <- read_csv("datasets/children_per_woman_total_fertility.csv")%>%
  select(country, `2009`) %>% rename('fertility' = `2009`)
x4 <- read_csv("datasets/co2_emissions_tonnes_per_person.csv") %>%
  select(country, `2009`) %>% rename('c02_emissions' = `2009`)
x5 <- read_csv("datasets/gdppercapita_us_inflation_adjusted.csv") %>%
  select(country, `2009`) %>% rename('gdp_per_capita' = `2009`)
x6 <- read_csv("datasets/income_per_person_gdppercapita_ppp_inflation_adjusted.csv") %>%
  select(country, `2009`) %>% rename('income_per_person' = `2009`)
x7 <- read_csv("datasets/life_expectancy_years.csv") %>%
  select(country, `2009`) %>% rename('life_expectancy' = `2009`)
x8 <- read_csv("datasets/murder_total_deaths.csv") %>%
  select(country, `2009`) %>% rename('murders' = `2009`)
x9 <- read_csv("datasets/population_density_per_square_km.csv") %>%
  select(country, `2009`) %>% rename('population_density' = `2009`)
x10 <- read_csv("datasets/population_total.csv") %>%
  select(country, `2009`) %>% rename('population' = `2009`)
x11 <- read_csv("datasets/total_health_spending_per_person_us.csv") %>%
  select(country, `2009`) %>% rename('health_spending' = `2009`)
x12 <- gapminder %>% select(country,continent) %>% distinct()
x12$country <- x12$country %>% str_replace('Yemen, Rep.', 'Yemen')
```

## Merging Data

``` r
dslist <- list(x12,x1,x2,x3,x4,x5,x6,x7,x8,x9,x10,x11)
df_merged <- dslist %>%
  reduce(full_join, by = 'country') %>% drop_na()
```

## Cleaning ‘k’ ‘M’ ‘B’

``` r
df <- df_merged %>% mutate(gdp_per_capita = case_when(
  str_detect(gdp_per_capita, 'k') == TRUE ~ as.numeric(str_remove(gdp_per_capita,'k')) * 1e3,
  str_detect(gdp_per_capita, 'M') == TRUE ~ as.numeric(str_remove(gdp_per_capita,'M')) * 1e6,
  str_detect(gdp_per_capita, 'B') == TRUE ~ as.numeric(str_remove(gdp_per_capita,'B')) * 1e9,
  TRUE ~ as.numeric(gdp_per_capita)) %>% format(scientific = FALSE),
  income_per_person = case_when(
  str_detect(income_per_person, 'k') == TRUE ~ as.numeric(str_remove(income_per_person,'k')) * 1e3,
  str_detect(income_per_person, 'M') == TRUE ~ as.numeric(str_remove(income_per_person,'M')) * 1e6,
  str_detect(income_per_person, 'B') == TRUE ~ as.numeric(str_remove(income_per_person,'B')) * 1e9,
  TRUE ~ as.numeric(income_per_person)) %>% format(scientific = FALSE),
  population = case_when(
  str_detect(population, 'k') == TRUE ~ as.numeric(str_remove(population,'k')) * 1e3,
  str_detect(population, 'M') == TRUE ~ as.numeric(str_remove(population,'M')) * 1e6,
  str_detect(population, 'B') == TRUE ~ as.numeric(str_remove(population,'B')) * 1e9,
  TRUE ~ as.numeric(population)) %>% format(scientific = FALSE))
```

    ## Warning: There were 10 warnings in `mutate()`.
    ## The first warning was:
    ## i In argument: `gdp_per_capita = `%>%`(...)`.
    ## Caused by warning:
    ## ! NAs introduced by coercion
    ## i Run `dplyr::last_dplyr_warnings()` to see the 9 remaining warnings.
