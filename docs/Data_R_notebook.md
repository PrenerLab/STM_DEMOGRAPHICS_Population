Research Data
================
Carter Hanford
(August 30, 2019)

Introduction
------------

This R notebook is for looking at decennial census race data from 1940-1990.

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
race1940 <- read.csv(here("NHGIS Data", "Decennial 1940", "nhgis0001_ds76_1940_tract.csv"))
race1950 <- read.csv(here("NHGIS Data", "Decennial 1950", "nhgis0002_ds82_1950_tract.csv"))
race1960 <- read.csv(here("NHGIS Data", "Decennial 1960", "nhgis0006_ds92_1960_tract.csv"))
race1970 <- read.csv(here("NHGIS Data", "Decennial 1970", "nhgis0005_ds98_1970_tract.csv"))
race1980 <- read.csv(here("NHGIS Data", "Decennial 1980", "nhgis0003_ds116_1980_tract.csv"))
race1990 <- read.csv(here("NHGIS Data", "Decennial 1990", "nhgis0004_ds120_1990_tract.csv"))
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

### 1990

``` r
race1990 %>%
  filter(STATE == "Missouri") -> race1990
```

``` r
race1990 %>%
  filter(COUNTY == "St Louis City" | COUNTY == "St Louis") -> race1990
```