Research Data
================
Carter Hanford
(September 06, 2019)

Introduction
------------

This R notebook is for looking at decennial census race data from 1940-1990 for a research project on redlining in the city of St. Louis.

Dependencies
------------

This notebook requires the following packages:

``` r
library(here)       # directory
library(readr)      # read .csv files
library(dplyr)      # data manipulation
library(tidyverse)  # data cleaning 
library(knitr)      # knit to documents
library(ggplot2)    # data plotting
```

Load Data
---------

The following code chunk will load each decennial census data year into our global environment.

``` r
race1940 <- read.csv(here("data", "raw", "race1940.csv"))
race1950 <- read.csv(here("data", "raw", "race1950.csv"))
race1960 <- read.csv(here("data", "raw", "race1960.csv"))
race1970 <- read.csv(here("data", "raw", "race1970.csv"))
race1980 <- read.csv(here("data", "raw", "race1980.csv"))
```

Select Census Tracts
--------------------

For this project we only want the St. Louis City and St. Louis County census tracts, so we will need to filter the data by state and by census tract so that we only have Missouri, and the correct tracts.

We will start with the year 1940.

### 1940

``` r
race1940 %>%
  filter(STATE == "Missouri") -> race1940
```

Now we will filter so we only have the correct census tracts.

``` r
race1940 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1940
```

Now 'race1940' is properly filtered.

Since i'll be repeating the same code in the following lines, I won't add narration.

### 1950

``` r
race1950 %>%
  filter(STATE == "Missouri") -> race1950
```

``` r
race1950 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1950
```

### 1960

``` r
race1960 %>%
  filter(STATE == "Missouri") -> race1960
```

``` r
race1960 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1960
```

### 1970

``` r
race1970 %>%
  filter(STATE == "Missouri") -> race1970
```

``` r
race1970 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1970
```

### 1980

``` r
race1980 %>%
  filter(STATE == "Missouri") -> race1980
```

``` r
race1980 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1980
```

Select/Rename Variables
-----------------------

It is unclear by the current distinction which variables represent `Race`, but for example, we know them to be `BUQ001` and `BUQ002` in the `race1940` data set. I will know change the variable names for each data set, starting with 1940, to represent the correct racial variables we are looking for. The code gets repetive for this section as well, so I will leave out narrative in between sections.

``` r
race1940 %>%
  rename(white = BUQ001) %>%
  rename(nonwhite = BUQ002) -> race1940
```

``` r
race1950 %>%
  rename(white = B0J001) %>%
  rename(black = B0J002) %>%
  rename(other = B0J003) -> race1950
```

``` r
race1960 %>%
  rename(white = B7B001) %>%
  rename(black = B7B002) %>%
  rename(other = B7B003) -> race1960
```

``` r
race1970 %>%
  rename(white = C0X001) %>%
  rename(black = C0X002) %>%
  rename(other = C0X003) -> race1970
```

``` r
race1980 %>%
  rename(white = C6X001) %>%
  rename(black = C6X002) -> race1980
```

The 1980 census has more specific instances of race. `C6X003`, `C6X004`, and `C6X005` represent American Indian/Eskimo/Aleut, Asian/Pacific Islander, and Other, respectively, so I combined them into one single variable called `other`.

``` r
race1980$other <- race1980$C6X003 + race1980$C6X004 + race1980$C6X005
```

Now that I have the race variables characterized intuitively, we can clean up the data sets by removing unnecessary variables. The only variables we want to keep are: `YEAR`, `COUNTYA`, `TRACTA`, `POSTTRACTA`, and the race variables.

Similar to above, the code gets repetive so I'll refrain from narrative after the first section.

``` r
race1940 %>%
  select(-STATE, -COUNTY, -STATEA, -PRETRACTA, -AREANAME, -GISJOIN) -> race1940
```

``` r
race1950 %>%
  select(-STATE, -COUNTY, -STATEA, -PRETRACTA, -AREANAME, -GISJOIN) -> race1950
```

``` r
race1960 %>%
  select(-STATE, -COUNTY, -STATEA, -PRETRACTA, -AREANAME, -GISJOIN) -> race1960
```

``` r
race1970 %>%
  select(-STATE, -COUNTY, -STATEA, -CTY_SUBA, -PLACEA, -SCSAA, -SMSAA, -URB_AREAA, -BLOCKA, -CDA, -GISJOIN, -AREANAME) -> race1970
```

``` r
race1980 %>%
  select(-STATE, -COUNTY, -STATEA, -CTY_SUBA, -PLACEA, -ELECPRCTA, -BLOCKA, -BLCK_GRPA, -ENUMDISTA, -C6X003, -C6X004, -C6X005, -GISJOIN, -AREANAME) -> race1980
```

Join Variables
--------------

Now we want to join `TRACTA` and `POSTTRCTA` together into one single variable. We can do this for the years 1940, 1950, and 1960, however 1970 does not have a `POSTTRACTA`. For this reason, I've left the tract information the data set and will not be joining any variables.

``` r
race1940 %>%
  unite("tractID", c("TRACTA", "POSTTRCTA")) -> race1940
```

``` r
race1950 %>%
  unite("tractID", c("TRACTA", "POSTTRCTA")) -> race1950
```

``` r
race1960 %>%
  unite("tractID", c("TRACTA", "POSTTRCTA")) -> race1960
```

Now that those are joined, the last thing I want to do is to convert all variables to lowercase and specific names for organizational purposes.

``` r
race1940 %>%
  rename(year = YEAR) %>%
  rename(countyID = COUNTYA) -> race1940
```

``` r
race1950 %>%
  rename(year = YEAR) %>%
  rename(countyID = COUNTYA) -> race1950
```

``` r
race1960 %>%
  rename(year = YEAR) %>%
  rename(countyID = COUNTYA) -> race1960
```

``` r
race1970 %>%
  rename(year = YEAR) %>%
  rename(countyID = COUNTYA) %>%
  rename(tractID = TRACTA) -> race1970
```

``` r
race1980 %>%
  rename(year = YEAR) %>%
  rename(countyID = COUNTYA) %>%
  rename(tractID = TRACTA) -> race1980
```

Now, for organizational purposes I want to get rid of the underscores in the new joined variable for tracts. This only applies for the years 1940-1960.

``` r
race1940 %>%
  mutate(tractID=str_replace(tractID, pattern = "_", replacement = "")) -> race1940
```

``` r
race1950 %>%
  mutate(tractID=str_replace(tractID, pattern = "_", replacement = "")) -> race1950
```

``` r
race1960 %>%
  mutate(tractID=str_replace(tractID, pattern = "_", replacement = "")) -> race1960
```

Data Export
-----------

Finally, we can export the cleaned census data into our clean data folder in the project directory.

``` r
write_csv(race1940, here("data", "clean", "race1940clean.csv"))
write_csv(race1950, here("data", "clean", "race1950clean.csv"))
write_csv(race1960, here("data", "clean", "race1960clean.csv"))
write_csv(race1970, here("data", "clean", "race1970clean.csv"))
write_csv(race1980, here("data", "clean", "race1980clean.csv"))
```
