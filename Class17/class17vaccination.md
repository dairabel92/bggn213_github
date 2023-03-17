Class 17 vaccination
================
dairabel

# Background

For todays class we will explore the data set on state wide vaccination
rates from CA.Gov

The goal of this hands-on mini-project is to examine and compare the
Covid-19 vaccination rates around San Diego.

We will start by downloading the most recently dated “Statewide COVID-19
Vaccines Administered by ZIP Code” CSV file from:
https://data.ca.gov/dataset/covid-19-vaccine-progress-dashboard-data-by-zip-code

## Data entry

``` r
vax <- read.csv("covid19vaccinesbyzipcode_test.csv")
head(vax)
```

      as_of_date zip_code_tabulation_area local_health_jurisdiction          county
    1 2021-01-05                    93609                    Fresno          Fresno
    2 2021-01-05                    94086               Santa Clara     Santa Clara
    3 2021-01-05                    94304               Santa Clara     Santa Clara
    4 2021-01-05                    94110             San Francisco   San Francisco
    5 2021-01-05                    93420           San Luis Obispo San Luis Obispo
    6 2021-01-05                    93454             Santa Barbara   Santa Barbara
      vaccine_equity_metric_quartile                 vem_source
    1                              1 Healthy Places Index Score
    2                              4 Healthy Places Index Score
    3                              4 Healthy Places Index Score
    4                              4 Healthy Places Index Score
    5                              3 Healthy Places Index Score
    6                              2 Healthy Places Index Score
      age12_plus_population age5_plus_population tot_population
    1                4396.3                 4839           5177
    2               42696.0                46412          50477
    3                3263.5                 3576           3852
    4               64350.7                68320          72380
    5               26694.9                29253          30740
    6               32043.4                36446          40432
      persons_fully_vaccinated persons_partially_vaccinated
    1                       NA                           NA
    2                       11                          640
    3                       NA                           NA
    4                       18                         1262
    5                       NA                           NA
    6                       NA                           NA
      percent_of_population_fully_vaccinated
    1                                     NA
    2                               0.000218
    3                                     NA
    4                               0.000249
    5                                     NA
    6                                     NA
      percent_of_population_partially_vaccinated
    1                                         NA
    2                                   0.012679
    3                                         NA
    4                                   0.017436
    5                                         NA
    6                                         NA
      percent_of_population_with_1_plus_dose booster_recip_count
    1                                     NA                  NA
    2                               0.012897                  NA
    3                                     NA                  NA
    4                               0.017685                  NA
    5                                     NA                  NA
    6                                     NA                  NA
      bivalent_dose_recip_count eligible_recipient_count
    1                        NA                        1
    2                        NA                       11
    3                        NA                        6
    4                        NA                       18
    5                        NA                        4
    6                        NA                        5
                                                                   redacted
    1 Information redacted in accordance with CA state privacy requirements
    2 Information redacted in accordance with CA state privacy requirements
    3 Information redacted in accordance with CA state privacy requirements
    4 Information redacted in accordance with CA state privacy requirements
    5 Information redacted in accordance with CA state privacy requirements
    6 Information redacted in accordance with CA state privacy requirements

> Q1. What column details the total number of people fully vaccinated?

``` r
head(vax$persons_fully_vaccinated)
```

    [1] NA 11 NA 18 NA NA

A: The column for total number of people fully vaccinated is the
persons_fully_vaccinated column.

> Q2. What column details the Zip code tabulation area?

``` r
head(vax$zip_code_tabulation_area)
```

    [1] 93609 94086 94304 94110 93420 93454

A: The second column, the zip_code_tabulation_area

> Q3. What is the earliest date in this dataset?

``` r
head(vax$as_of_date[1])
```

    [1] "2021-01-05"

A: The earliest was on January 5th, 2021.

> Q4. What is the latest date in this dataset?

``` r
tail(vax$as_of_date)
```

    [1] "2023-03-07" "2023-03-07" "2023-03-07" "2023-03-07" "2023-03-07"
    [6] "2023-03-07"

A: Last one is March 07, 2023.

``` r
vax$as_of_date[nrow(vax)]
```

    [1] "2023-03-07"

A: confirm it, march 7, 2023!

Useful function for exploring the data! use skimr package **skimr**

``` r
library(skimr)
skim(vax)
```

|                                                  |        |
|:-------------------------------------------------|:-------|
| Name                                             | vax    |
| Number of rows                                   | 201096 |
| Number of columns                                | 18     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |        |
| Column type frequency:                           |        |
| character                                        | 5      |
| numeric                                          | 13     |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |        |
| Group variables                                  | None   |

Data summary

**Variable type: character**

| skim_variable             | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| as_of_date                |         0 |             1 |  10 |  10 |     0 |      114 |          0 |
| local_health_jurisdiction |         0 |             1 |   0 |  15 |   570 |       62 |          0 |
| county                    |         0 |             1 |   0 |  15 |   570 |       59 |          0 |
| vem_source                |         0 |             1 |  15 |  26 |     0 |        3 |          0 |
| redacted                  |         0 |             1 |   2 |  69 |     0 |        2 |          0 |

**Variable type: numeric**

| skim_variable                              | n_missing | complete_rate |     mean |       sd |    p0 |      p25 |      p50 |      p75 |     p100 | hist  |
|:-------------------------------------------|----------:|--------------:|---------:|---------:|------:|---------:|---------:|---------:|---------:|:------|
| zip_code_tabulation_area                   |         0 |          1.00 | 93665.11 |  1817.38 | 90001 | 92257.75 | 93658.50 | 95380.50 |  97635.0 | ▃▅▅▇▁ |
| vaccine_equity_metric_quartile             |      9918 |          0.95 |     2.44 |     1.11 |     1 |     1.00 |     2.00 |     3.00 |      4.0 | ▇▇▁▇▇ |
| age12_plus_population                      |         0 |          1.00 | 18895.04 | 18993.87 |     0 |  1346.95 | 13685.10 | 31756.12 |  88556.7 | ▇▃▂▁▁ |
| age5_plus_population                       |         0 |          1.00 | 20875.24 | 21105.97 |     0 |  1460.50 | 15364.00 | 34877.00 | 101902.0 | ▇▃▂▁▁ |
| tot_population                             |      9804 |          0.95 | 23372.77 | 22628.50 |    12 |  2126.00 | 18714.00 | 38168.00 | 111165.0 | ▇▅▂▁▁ |
| persons_fully_vaccinated                   |     16621 |          0.92 | 13990.39 | 15073.66 |    11 |   932.00 |  8589.00 | 23346.00 |  87575.0 | ▇▃▁▁▁ |
| persons_partially_vaccinated               |     16621 |          0.92 |  1702.31 |  2033.32 |    11 |   165.00 |  1197.00 |  2536.00 |  39973.0 | ▇▁▁▁▁ |
| percent_of_population_fully_vaccinated     |     20965 |          0.90 |     0.57 |     0.25 |     0 |     0.42 |     0.61 |     0.74 |      1.0 | ▂▃▆▇▃ |
| percent_of_population_partially_vaccinated |     20965 |          0.90 |     0.08 |     0.09 |     0 |     0.05 |     0.06 |     0.08 |      1.0 | ▇▁▁▁▁ |
| percent_of_population_with_1\_plus_dose    |     22009 |          0.89 |     0.63 |     0.24 |     0 |     0.49 |     0.67 |     0.81 |      1.0 | ▂▂▅▇▆ |
| booster_recip_count                        |     72997 |          0.64 |  5882.76 |  7219.00 |    11 |   300.00 |  2773.00 |  9510.00 |  59593.0 | ▇▂▁▁▁ |
| bivalent_dose_recip_count                  |    158776 |          0.21 |  2978.23 |  3633.03 |    11 |   193.00 |  1467.50 |  4730.25 |  27694.0 | ▇▂▁▁▁ |
| eligible_recipient_count                   |         0 |          1.00 | 12830.83 | 14928.64 |     0 |   507.00 |  6369.00 | 22014.00 |  87248.0 | ▇▃▁▁▁ |

``` r
#or any package
#skimr::skim(vax)
```

> Q5. How many numeric columns are in this dataset?

A: there are 13 numeric, based on skmir results

> Q6. Note that there are “missing values” in the dataset. How many NA
> values there in the persons_fully_vaccinated column?

``` r
n.missing <- sum(is.na(vax$persons_fully_vaccinated))
n.missing
```

    [1] 16621

A: There are 16621 NA values in the persons_fully_vaccinated column

> Q7. What percent of persons_fully_vaccinated values are missing (to 2
> significant figures)?

``` r
round(n.missing/nrow(vax) * 100, 2)
```

    [1] 8.27

A: 8.27 % are missing.

> Q8\[Optional\]: Why might this data be missing?

A: One reason is that it was at the early stages of vaccination efforts
and the tracking wasn’t standard or it was new. it coudl also be human
error and fault in tracking.

## Working with the dates

We will use the **lubridate** package to help ease the pain of wrokig
with times and dates

``` r
library(lubridate)
```


    Attaching package: 'lubridate'

    The following objects are masked from 'package:base':

        date, intersect, setdiff, union

``` r
today()
```

    [1] "2023-03-17"

``` r
today() -ymd("1992-8-10")
```

    Time difference of 11176 days

``` r
today() -ymd("2021-6-28")
```

    Time difference of 627 days

> Q9. How many days have passed since the last update of the dataset?

I will conver the entire “as_of_date” column to be in lubridate format

``` r
vax$as_of_date <- ymd(vax$as_of_date)
```

``` r
today() - vax$as_of_date[nrow(vax)]
```

    Time difference of 10 days

A: 1 day!

> Q10. How many unique dates are in the dataset (i.e. how many different
> dates are detailed)?

``` r
unique <- length(unique(vax$as_of_date))
unique
```

    [1] 114

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
n_distinct(vax$as_of_date)
```

    [1] 114

A: There are 114 unique dates in the dataset

# Working with zip codes

There are quite a few R packages that can help ease the pain of working
with zip codes. we will try just one the smmler ones here called
**zipcodeR** install it

``` r
library(zipcodeR)
```

``` r
geocode_zip('92037')
```

    # A tibble: 1 × 3
      zipcode   lat   lng
      <chr>   <dbl> <dbl>
    1 92037    32.8 -117.

``` r
geocode_zip('89509')
```

    # A tibble: 1 × 3
      zipcode   lat   lng
      <chr>   <dbl> <dbl>
    1 89509    39.5 -120.

``` r
zip_distance('92037','92109')
```

      zipcode_a zipcode_b distance
    1     92037     92109     2.33

``` r
reverse_zipcode(c('92037', "92109", "89509") )
```

    # A tibble: 3 × 24
      zipcode zipcode_…¹ major…² post_…³ common_c…⁴ county state   lat   lng timez…⁵
      <chr>   <chr>      <chr>   <chr>       <blob> <chr>  <chr> <dbl> <dbl> <chr>  
    1 92037   Standard   La Jol… La Jol… <raw 20 B> San D… CA     32.8 -117. Pacific
    2 92109   Standard   San Di… San Di… <raw 21 B> San D… CA     32.8 -117. Pacific
    3 89509   Standard   Reno    Reno, … <raw 16 B> Washo… NV     39.5 -120. Pacific
    # … with 14 more variables: radius_in_miles <dbl>, area_code_list <blob>,
    #   population <int>, population_density <dbl>, land_area_in_sqmi <dbl>,
    #   water_area_in_sqmi <dbl>, housing_units <int>,
    #   occupied_housing_units <int>, median_home_value <int>,
    #   median_household_income <int>, bounds_west <dbl>, bounds_east <dbl>,
    #   bounds_north <dbl>, bounds_south <dbl>, and abbreviated variable names
    #   ¹​zipcode_type, ²​major_city, ³​post_office_city, ⁴​common_city_list, …

> Q FInd the best and worst ratio of “median household income” in San
> Diego

first we need to get san diego isolated!

# Focus on San Diego

``` r
# Subset to San Diego county only areas
sd <- vax[vax$county=="San Diego" , ]
sd.zip <- unique(vax$zip_code_tabulation_area[vax$county=="San Diego"])
length(sd.zip)
```

    [1] 107

Now do azipcode look up for the data we want

``` r
sd.eco <- reverse_zipcode(sd.zip)
```

Now extract the “median household income” and “median home value”

Most expensive one?

``` r
ord <- order(sd.eco$median_home_value, decreasing =TRUE) 
ord
```

      [1]  33  44  57  85 103  68  61  74  96  28  77  37  32  75  30  97  93  29
     [19]  95  89  88  71  26  91  15  54   2  94  16  42  31   1  78   8  87  69
     [37]  84  83  64  72  46  86  41  34  38  90  66  63   7  79  55  35  92  62
     [55]   9  82  58  59  39  53  48  17  24  49  13  45   5  21  60  43  40  56
     [73]  36  51  23   6  18  76  65  70   3 101  81 105  73  20  50  10 107  80
     [91]  27   4  11  67  14  22  12  19  25  47  52  98  99 100 102 104 106

``` r
head(sd.eco[ord,])
```

    # A tibble: 6 × 24
      zipcode zipcode_…¹ major…² post_…³ common_c…⁴ county state   lat   lng timez…⁵
      <chr>   <chr>      <chr>   <chr>       <blob> <chr>  <chr> <dbl> <dbl> <chr>  
    1 92014   Standard   Del Mar Del Ma… <raw 19 B> San D… CA     33.0 -117. Pacific
    2 92037   Standard   La Jol… La Jol… <raw 20 B> San D… CA     32.8 -117. Pacific
    3 92067   PO Box     Rancho… Rancho… <raw 33 B> San D… CA     33.0 -117. Pacific
    4 92118   Standard   Corona… Corona… <raw 33 B> San D… CA     32.6 -117. Pacific
    5 92145   Unique     San Di… San Di… <raw 21 B> San D… CA     32.9 -117. Pacific
    6 92091   Standard   Rancho… Rancho… <raw 33 B> San D… CA     33   -117. Pacific
    # … with 14 more variables: radius_in_miles <dbl>, area_code_list <blob>,
    #   population <int>, population_density <dbl>, land_area_in_sqmi <dbl>,
    #   water_area_in_sqmi <dbl>, housing_units <int>,
    #   occupied_housing_units <int>, median_home_value <int>,
    #   median_household_income <int>, bounds_west <dbl>, bounds_east <dbl>,
    #   bounds_north <dbl>, bounds_south <dbl>, and abbreviated variable names
    #   ¹​zipcode_type, ²​major_city, ³​post_office_city, ⁴​common_city_list, …

``` r
head(arrange(sd.eco, desc(median_home_value)))
```

    # A tibble: 6 × 24
      zipcode zipcode_…¹ major…² post_…³ common_c…⁴ county state   lat   lng timez…⁵
      <chr>   <chr>      <chr>   <chr>       <blob> <chr>  <chr> <dbl> <dbl> <chr>  
    1 92014   Standard   Del Mar Del Ma… <raw 19 B> San D… CA     33.0 -117. Pacific
    2 92037   Standard   La Jol… La Jol… <raw 20 B> San D… CA     32.8 -117. Pacific
    3 92067   PO Box     Rancho… Rancho… <raw 33 B> San D… CA     33.0 -117. Pacific
    4 92118   Standard   Corona… Corona… <raw 33 B> San D… CA     32.6 -117. Pacific
    5 92145   Unique     San Di… San Di… <raw 21 B> San D… CA     32.9 -117. Pacific
    6 92091   Standard   Rancho… Rancho… <raw 33 B> San D… CA     33   -117. Pacific
    # … with 14 more variables: radius_in_miles <dbl>, area_code_list <blob>,
    #   population <int>, population_density <dbl>, land_area_in_sqmi <dbl>,
    #   water_area_in_sqmi <dbl>, housing_units <int>,
    #   occupied_housing_units <int>, median_home_value <int>,
    #   median_household_income <int>, bounds_west <dbl>, bounds_east <dbl>,
    #   bounds_north <dbl>, bounds_south <dbl>, and abbreviated variable names
    #   ¹​zipcode_type, ²​major_city, ³​post_office_city, ⁴​common_city_list, …

I will use dyplr to help me do more involved selections (ie filter rows
to include the subset of data we are interested in)

``` r
library(dplyr)

sd <- filter(vax, county == "San Diego")

nrow(sd)
```

    [1] 12198

``` r
sd.10 <- filter(vax, county == "San Diego" &
                age5_plus_population > 10000)
```

> Q11. How many distinct zip codes are listed for San Diego County?

``` r
length(unique(sd$zip_code_tabulation_area))
```

    [1] 107

A: There are 107 zip codes

> Q12. What San Diego County Zip code area has the largest 12 +
> Population in this dataset?

``` r
sd.12 <- which.max(sd$age12_plus_population)
sd[sd.12,]
```

       as_of_date zip_code_tabulation_area local_health_jurisdiction    county
    67 2021-01-05                    92154                 San Diego San Diego
       vaccine_equity_metric_quartile                 vem_source
    67                              2 Healthy Places Index Score
       age12_plus_population age5_plus_population tot_population
    67               76365.2                82971          88979
       persons_fully_vaccinated persons_partially_vaccinated
    67                       16                         1400
       percent_of_population_fully_vaccinated
    67                                0.00018
       percent_of_population_partially_vaccinated
    67                                   0.015734
       percent_of_population_with_1_plus_dose booster_recip_count
    67                               0.015914                  NA
       bivalent_dose_recip_count eligible_recipient_count
    67                        NA                       16
                                                                    redacted
    67 Information redacted in accordance with CA state privacy requirements

A: the zip code is 92154

> Q13. What is the overall average “Percent of Population Fully
> Vaccinated” value for all San Diego “County” as of “2023-03-07”?

``` r
vax$as_of_date[nrow(vax)]
```

    [1] "2023-03-07"

``` r
sd.latest <- filter(sd, as_of_date=="2023-03-07")
head(sd.latest)
```

      as_of_date zip_code_tabulation_area local_health_jurisdiction    county
    1 2023-03-07                    92124                 San Diego San Diego
    2 2023-03-07                    92026                 San Diego San Diego
    3 2023-03-07                    91910                 San Diego San Diego
    4 2023-03-07                    91978                 San Diego San Diego
    5 2023-03-07                    92114                 San Diego San Diego
    6 2023-03-07                    91962                 San Diego San Diego
      vaccine_equity_metric_quartile                 vem_source
    1                              3 Healthy Places Index Score
    2                              2 Healthy Places Index Score
    3                              2 Healthy Places Index Score
    4                              2 Healthy Places Index Score
    5                              2 Healthy Places Index Score
    6                              3 Healthy Places Index Score
      age12_plus_population age5_plus_population tot_population
    1               25422.4                29040          32600
    2               42613.9                46283          50321
    3               64013.6                70086          74855
    4                8644.9                 9663          10506
    5               59050.7                64945          68851
    6                1758.7                 2020           2106
      persons_fully_vaccinated persons_partially_vaccinated
    1                    18864                         2490
    2                    35817                         3010
    3                    74079                        16143
    4                     7007                          703
    5                    50624                         5337
    6                     1031                           61
      percent_of_population_fully_vaccinated
    1                               0.578650
    2                               0.711770
    3                               0.989633
    4                               0.666952
    5                               0.735269
    6                               0.489554
      percent_of_population_partially_vaccinated
    1                                   0.076380
    2                                   0.059816
    3                                   0.215657
    4                                   0.066914
    5                                   0.077515
    6                                   0.028965
      percent_of_population_with_1_plus_dose booster_recip_count
    1                               0.655030               12151
    2                               0.771586               21320
    3                               1.000000               43124
    4                               0.733866                3981
    5                               0.812784               30156
    6                               0.518519                 593
      bivalent_dose_recip_count eligible_recipient_count redacted
    1                      5632                    18702       No
    2                      7776                    35716       No
    3                     13760                    73816       No
    4                      1284                     6988       No
    5                      9497                    50449       No
    6                       222                     1028       No

``` r
mean(sd.latest$percent_of_population_fully_vaccinated, na.rm=TRUE)
```

    [1] 0.7402567

A: 74%

# now to make some figures\~

> Q14. Using either ggplot or base R graphics make a summary figure that
> shows the distribution of Percent of Population Fully Vaccinated
> values as of “2023-03-07”

``` r
hist(sd.latest$percent_of_population_fully_vaccinated)
```

![](class17vaccination_files/figure-commonmark/unnamed-chunk-34-1.png)

use ggplot2 baby!

``` r
library(ggplot2)
```

``` r
ggplot(sd.latest) +
  aes(percent_of_population_fully_vaccinated) +
  geom_histogram()
```

    `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    Warning: Removed 8 rows containing non-finite values (`stat_bin()`).

![](class17vaccination_files/figure-commonmark/unnamed-chunk-36-1.png)

``` r
ggplot(sd.latest) +
  aes(percent_of_population_fully_vaccinated) +
  geom_histogram(bins=20) +
  xlab("Percent of People Fully vaccinated")
```

    Warning: Removed 8 rows containing non-finite values (`stat_bin()`).

![](class17vaccination_files/figure-commonmark/unnamed-chunk-37-1.png)

# Focus on UCSD/La Jolla

UC San Diego resides in the 92037 ZIP code area and is listed with an
age 5+ population size of 36,144.

``` r
ucsd <- filter(sd, zip_code_tabulation_area=="92037")
ucsd[1,]$age5_plus_population
```

    [1] 36144

Make a time plot!

> Q15. Using ggplot make a graph of the vaccination rate time course for
> the 92037 ZIP code area:

``` r
lj <- ggplot(ucsd) +
  aes(as_of_date,
      percent_of_population_fully_vaccinated) +
  geom_point() +
  geom_line(group=1) +
  ylim(c(0,1)) +
  labs(x= "Date", y="Percent Vaccinated", title="vaccination rate for La Jolla")
lj
```

![](class17vaccination_files/figure-commonmark/unnamed-chunk-39-1.png)

# Comparing to similar sizes

Let’s return to the full dataset and look across every zip code area
with a population at least as large as that of 92037 on as_of_date
“2023-03-07”.

``` r
# Subset to all CA areas with a population as large as 92037
vax.36 <- filter(vax, age5_plus_population > 36144 &
                as_of_date == "2023-03-07")

head(vax.36)
```

      as_of_date zip_code_tabulation_area local_health_jurisdiction         county
    1 2023-03-07                    94116             San Francisco  San Francisco
    2 2023-03-07                    92703                    Orange         Orange
    3 2023-03-07                    94118             San Francisco  San Francisco
    4 2023-03-07                    92376            San Bernardino San Bernardino
    5 2023-03-07                    92692                    Orange         Orange
    6 2023-03-07                    95148               Santa Clara    Santa Clara
      vaccine_equity_metric_quartile                 vem_source
    1                              4 Healthy Places Index Score
    2                              1 Healthy Places Index Score
    3                              4 Healthy Places Index Score
    4                              1 Healthy Places Index Score
    5                              4 Healthy Places Index Score
    6                              4 Healthy Places Index Score
      age12_plus_population age5_plus_population tot_population
    1               42334.3                45160          47346
    2               57182.7                64387          69112
    3               37628.5                40012          42095
    4               70232.1                79686          86085
    5               41008.9                44243          46800
    6               42163.3                46202          48273
      persons_fully_vaccinated persons_partially_vaccinated
    1                    41255                         2450
    2                    57887                         7399
    3                    33284                         3040
    4                    51367                         5674
    5                    35117                         2603
    6                    42298                         2684
      percent_of_population_fully_vaccinated
    1                               0.871351
    2                               0.837582
    3                               0.790688
    4                               0.596701
    5                               0.750363
    6                               0.876225
      percent_of_population_partially_vaccinated
    1                                   0.051747
    2                                   0.107058
    3                                   0.072218
    4                                   0.065912
    5                                   0.055620
    6                                   0.055600
      percent_of_population_with_1_plus_dose booster_recip_count
    1                               0.923098               34108
    2                               0.944640               28297
    3                               0.862906               27401
    4                               0.662613               23832
    5                               0.805983               23695
    6                               0.931825               31583
      bivalent_dose_recip_count eligible_recipient_count redacted
    1                     19158                    41000       No
    2                      7627                    57775       No
    3                     15251                    33146       No
    4                      6393                    51276       No
    5                     10169                    35031       No
    6                     12604                    42120       No

> randomd Q How many zip codes areas are we talking about?

``` r
n_distinct(vax.36$zip_code_tabulation_area)
```

    [1] 411

> Q16. Calculate the mean “Percent of Population Fully Vaccinated” for
> ZIP code areas with a population as large as 92037 (La Jolla)
> as_of_date “2023-03-07”. Add this as a straight horizontal line to
> your plot from above with the geom_hline() function?

``` r
comparison <- mean(vax.36$percent_of_population_fully_vaccinated, na.rm=TRUE)
comparison
```

    [1] 0.7214936

``` r
lj + geom_hline(yintercept=comparison, color="red", linetype=2)
```

![](class17vaccination_files/figure-commonmark/unnamed-chunk-43-1.png)

> Q17. What is the 6 number summary (Min, 1st Qu., Median, Mean, 3rd
> Qu., and Max) of the “Percent of Population Fully Vaccinated” values
> for ZIP code areas with a population as large as 92037 (La Jolla)
> as_of_date “2023-03-07”?

``` r
summary(vax.36$percent_of_population_fully_vaccinated)
```

       Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
     0.3805  0.6459  0.7183  0.7215  0.7908  1.0000 

You use the summary function!

> Q18. Using ggplot generate a histogram of this data.

``` r
ggplot(vax.36) +
  aes(percent_of_population_fully_vaccinated)+
  geom_histogram()+
  xlim(0,1)+
  labs(x= "percent of people fully vaccinated", y="count", title="Histogram of Data")
```

    `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    Warning: Removed 2 rows containing missing values (`geom_bar()`).

![](class17vaccination_files/figure-commonmark/unnamed-chunk-45-1.png)

> Q19. Is the 92109 and 92040 ZIP code areas above or below the average
> value you calculated for all these above?

``` r
vax %>% filter(as_of_date == "2023-03-07") %>%  
  filter(zip_code_tabulation_area=="92040") %>%
  select(percent_of_population_fully_vaccinated)
```

      percent_of_population_fully_vaccinated
    1                               0.550533

``` r
vax %>% filter(as_of_date == "2023-03-07") %>%  
  filter(zip_code_tabulation_area=="92109") %>%
  select(percent_of_population_fully_vaccinated)
```

      percent_of_population_fully_vaccinated
    1                               0.694636

``` r
filter(vax.36, zip_code_tabulation_area %in% c("92109","92040"))
```

      as_of_date zip_code_tabulation_area local_health_jurisdiction    county
    1 2023-03-07                    92109                 San Diego San Diego
    2 2023-03-07                    92040                 San Diego San Diego
      vaccine_equity_metric_quartile                 vem_source
    1                              3 Healthy Places Index Score
    2                              3 Healthy Places Index Score
      age12_plus_population age5_plus_population tot_population
    1               43222.5                44953          47111
    2               39405.0                42833          46306
      persons_fully_vaccinated persons_partially_vaccinated
    1                    32725                         4234
    2                    25493                         2156
      percent_of_population_fully_vaccinated
    1                               0.694636
    2                               0.550533
      percent_of_population_partially_vaccinated
    1                                   0.089873
    2                                   0.046560
      percent_of_population_with_1_plus_dose booster_recip_count
    1                               0.784509               19677
    2                               0.597093               14175
      bivalent_dose_recip_count eligible_recipient_count redacted
    1                      8109                    32622       No
    2                      4649                    25433       No

A: both are below the average of .72

> Q20. Finally make a time course plot of vaccination progress for all
> areas in the full dataset with a age5_plus_population \> 36144.

``` r
vax.36.all <- filter(vax, age5_plus_population > 36144)


ggplot(vax.36.all) +
  aes(as_of_date,
      percent_of_population_fully_vaccinated, 
      group=zip_code_tabulation_area) +
  geom_line(alpha=0.2, color="blue") +
  ylim(0,1) +
  labs(x="date", y="percent vaccinated",
       title="Vaccination Rate across California",
       subtitle="only areas with population above 36k are shown") +
  geom_hline(yintercept = 0.72, linetype=5)
```

    Warning: Removed 183 rows containing missing values (`geom_line()`).

![](class17vaccination_files/figure-commonmark/unnamed-chunk-49-1.png)

> Q21. How do you feel about traveling for Spring Break and meeting for
> in-person class afterwards?

A: I feel fine about it! most people are alreadt vaccinated by this
timeppoint!
