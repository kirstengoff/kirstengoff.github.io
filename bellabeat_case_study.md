Case Study 2: Bellabeat
================
Kirsten Goff
2023-11-10

# How Can a Wellness Company Play It Smart?

#### Google Data Analytics Professional Certificate: Case Study 2

------------------------------------------------------------------------

### Introduction

Welcome! My name is Kirsten Goff and I can be contacted at
kirstenagoff@gmail.com. Please feel free to connect with me on
[**Linkedin**](https://www.linkedin.com/in/kirsten-goff-research/) and
view more of my work in my
[**Portfolio**](https://github.com/kirstengoff/kirstengoff.github.io).

You can also view this document as an html file
[**here**](https://a9ee73628c1b4ebc936b7f375ca13f19.app.posit.cloud/file_show?path=%2Fcloud%2Fproject%2Fscripts%2Fproduct%2Fbellabeat_case_study.html).

In this case study I will perform the real-world tasks of a data analyst
to answer key business questions for the company Bellabeat. I will use R
to follow the steps of the data analysis process: ask, prepare, process,
analyze, share, and act.

### Background (Ask)

#### <span style="color: darkblue;">Context</span>

Bellabeat is a small but successful high-tech manufacturer of
health-focused products for women. Since its founding in 2013, Bellabeat
has opened offices around the world, launched multiple products, and
become available through online retailers and their own e-commerce
channel. Their current marketing strategy includes traditional
advertising but focuses more extensively on digital marketing.

Bellabeat’s co-founder, Urška Sršen, believes that analyzing
non-Bellabeat smart device usage data and applying these insights to
Bellabeat’s products is the key to expanding in the global smart device
market. This presentation will focus on recommendations for their
product ‘Time’, a wellness watch that tracks user activity, sleep, and
stress.

#### <span style="color: darkblue;">The Problem</span>

How can non-Bellabeat smart device usage trends inform Bellabeat’s
marketing strategy for their product, ‘Time’?

#### <span style="color: darkblue;">Business Task</span>

1.  What are some trends in smart device usage?

2.  How could these trends apply to Bellabeat customers?

3.  How could the trends help influence Bellabeat marketing strategy?

### The Data (Prepare)

#### <span style="color: darkblue;">Sources</span>

As instructed by Bellabeat, I will be utilizing public data exploring
smart device users’ daily habits, specifically the [Fitbit Fitness
Tracker Data Set](https://www.kaggle.com/datasets/arashnic/fitbit). This
Kaggle data set, consisting of 18 CSV files, contains data from 30
Fitbit users who consented to the submission of their personal Fitbit
tracker data including heart rate, physical activity, and sleep
monitoring. Participants were recruited by responding to a survey
administered by Amazon Mechanical Turk, who collected the data between
2016-03-12 and 2016-05-12.

- Possible Limitations:

  - Small sample size (30 participants)

  - Data is over 6 years old

- Citation:

  - Furberg, R., Brinton, J., Keating, M., & Ortiz, A. (2016).
    Crowd-sourced Fitbit datasets 03.12.2016-05.12.2016 \[Data set\].
    Zenodo. <https://doi.org/10.5281/zenodo.53894>

#### <span style="color: darkblue;">Loading Data</span>

I installed and loaded R packages necessary for data cleaning, analysis,
and visualization. I utilized message=FALSE warning=FALSE to prevent
generation of warning messages.

Next, I imported and renamed all of the Fitbit data sets. I also
reviewed some key summary statistics.

- dailyActivity_merged.csv

  ``` r
  daily_activity <- read_csv(here("/cloud/project/fitbit_data/daily_activity.csv"),col_types = cols(ActivityDate = col_date(format = "%m/%d/%Y")))
    colnames(daily_activity)[2] <- "date"

     str(daily_activity)
  ```

      ## spc_tbl_ [940 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id                      : num [1:940] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
      ##  $ date                    : Date[1:940], format: "2016-04-12" "2016-04-13" ...
      ##  $ TotalSteps              : num [1:940] 13162 10735 10460 9762 12669 ...
      ##  $ TotalDistance           : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
      ##  $ TrackerDistance         : num [1:940] 8.5 6.97 6.74 6.28 8.16 ...
      ##  $ LoggedActivitiesDistance: num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
      ##  $ VeryActiveDistance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
      ##  $ ModeratelyActiveDistance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
      ##  $ LightActiveDistance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
      ##  $ SedentaryActiveDistance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
      ##  $ VeryActiveMinutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
      ##  $ FairlyActiveMinutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
      ##  $ LightlyActiveMinutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
      ##  $ SedentaryMinutes        : num [1:940] 728 776 1218 726 773 ...
      ##  $ Calories                : num [1:940] 1985 1797 1776 1745 1863 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   ActivityDate = col_date(format = "%m/%d/%Y"),
      ##   ..   TotalSteps = col_double(),
      ##   ..   TotalDistance = col_double(),
      ##   ..   TrackerDistance = col_double(),
      ##   ..   LoggedActivitiesDistance = col_double(),
      ##   ..   VeryActiveDistance = col_double(),
      ##   ..   ModeratelyActiveDistance = col_double(),
      ##   ..   LightActiveDistance = col_double(),
      ##   ..   SedentaryActiveDistance = col_double(),
      ##   ..   VeryActiveMinutes = col_double(),
      ##   ..   FairlyActiveMinutes = col_double(),
      ##   ..   LightlyActiveMinutes = col_double(),
      ##   ..   SedentaryMinutes = col_double(),
      ##   ..   Calories = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
     colnames(daily_activity)
  ```

      ##  [1] "Id"                       "date"                    
      ##  [3] "TotalSteps"               "TotalDistance"           
      ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
      ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
      ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
      ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
      ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
      ## [15] "Calories"

  ``` r
     head(daily_activity)
  ```

      ## # A tibble: 6 × 15
      ##           Id date       TotalSteps TotalDistance TrackerDistance
      ##        <dbl> <date>          <dbl>         <dbl>           <dbl>
      ## 1 1503960366 2016-04-12      13162          8.5             8.5 
      ## 2 1503960366 2016-04-13      10735          6.97            6.97
      ## 3 1503960366 2016-04-14      10460          6.74            6.74
      ## 4 1503960366 2016-04-15       9762          6.28            6.28
      ## 5 1503960366 2016-04-16      12669          8.16            8.16
      ## 6 1503960366 2016-04-17       9705          6.48            6.48
      ## # ℹ 10 more variables: LoggedActivitiesDistance <dbl>,
      ## #   VeryActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
      ## #   LightActiveDistance <dbl>, SedentaryActiveDistance <dbl>,
      ## #   VeryActiveMinutes <dbl>, FairlyActiveMinutes <dbl>,
      ## #   LightlyActiveMinutes <dbl>, SedentaryMinutes <dbl>, Calories <dbl>

  ``` r
     nrow(daily_activity)
  ```

      ## [1] 940

- sleepDay_merged.csv

  ``` r
  sleep_day <- read_csv(here("/cloud/project/fitbit_data/sleep_day.csv"))

    str(sleep_day)
  ```

      ## spc_tbl_ [413 × 5] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id                : num [1:413] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
      ##  $ SleepDay          : chr [1:413] "4/12/2016 12:00:00 AM" "4/13/2016 12:00:00 AM" "4/15/2016 12:00:00 AM" "4/16/2016 12:00:00 AM" ...
      ##  $ TotalSleepRecords : num [1:413] 1 2 1 2 1 1 1 1 1 1 ...
      ##  $ TotalMinutesAsleep: num [1:413] 327 384 412 340 700 304 360 325 361 430 ...
      ##  $ TotalTimeInBed    : num [1:413] 346 407 442 367 712 320 377 364 384 449 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   SleepDay = col_character(),
      ##   ..   TotalSleepRecords = col_double(),
      ##   ..   TotalMinutesAsleep = col_double(),
      ##   ..   TotalTimeInBed = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
    colnames(sleep_day)
  ```

      ## [1] "Id"                 "SleepDay"           "TotalSleepRecords" 
      ## [4] "TotalMinutesAsleep" "TotalTimeInBed"

  ``` r
    head(sleep_day)
  ```

      ## # A tibble: 6 × 5
      ##           Id SleepDay        TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
      ##        <dbl> <chr>                       <dbl>              <dbl>          <dbl>
      ## 1 1503960366 4/12/2016 12:0…                 1                327            346
      ## 2 1503960366 4/13/2016 12:0…                 2                384            407
      ## 3 1503960366 4/15/2016 12:0…                 1                412            442
      ## 4 1503960366 4/16/2016 12:0…                 2                340            367
      ## 5 1503960366 4/17/2016 12:0…                 1                700            712
      ## 6 1503960366 4/19/2016 12:0…                 1                304            320

  ``` r
    nrow(sleep_day)
  ```

      ## [1] 413

- weightLogInfo_merged.csv

  ``` r
  weight <- read_csv(here("/cloud/project/fitbit_data/weight.csv"))

    str(weight)
  ```

      ## spc_tbl_ [67 × 8] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id            : num [1:67] 1.50e+09 1.50e+09 1.93e+09 2.87e+09 2.87e+09 ...
      ##  $ Date          : chr [1:67] "5/2/2016 11:59:59 PM" "5/3/2016 11:59:59 PM" "4/13/2016 1:08:52 AM" "4/21/2016 11:59:59 PM" ...
      ##  $ WeightKg      : num [1:67] 52.6 52.6 133.5 56.7 57.3 ...
      ##  $ WeightPounds  : num [1:67] 116 116 294 125 126 ...
      ##  $ Fat           : num [1:67] 22 NA NA NA NA 25 NA NA NA NA ...
      ##  $ BMI           : num [1:67] 22.6 22.6 47.5 21.5 21.7 ...
      ##  $ IsManualReport: logi [1:67] TRUE TRUE FALSE TRUE TRUE TRUE ...
      ##  $ LogId         : num [1:67] 1.46e+12 1.46e+12 1.46e+12 1.46e+12 1.46e+12 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   Date = col_character(),
      ##   ..   WeightKg = col_double(),
      ##   ..   WeightPounds = col_double(),
      ##   ..   Fat = col_double(),
      ##   ..   BMI = col_double(),
      ##   ..   IsManualReport = col_logical(),
      ##   ..   LogId = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
    colnames(weight)
  ```

      ## [1] "Id"             "Date"           "WeightKg"       "WeightPounds"  
      ## [5] "Fat"            "BMI"            "IsManualReport" "LogId"

  ``` r
    head(weight)
  ```

      ## # A tibble: 6 × 8
      ##           Id Date       WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
      ##        <dbl> <chr>         <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
      ## 1 1503960366 5/2/2016 …     52.6         116.    22  22.6 TRUE           1.46e12
      ## 2 1503960366 5/3/2016 …     52.6         116.    NA  22.6 TRUE           1.46e12
      ## 3 1927972279 4/13/2016…    134.          294.    NA  47.5 FALSE          1.46e12
      ## 4 2873212765 4/21/2016…     56.7         125.    NA  21.5 TRUE           1.46e12
      ## 5 2873212765 5/12/2016…     57.3         126.    NA  21.7 TRUE           1.46e12
      ## 6 4319703577 4/17/2016…     72.4         160.    25  27.5 TRUE           1.46e12

  ``` r
    nrow(weight)
  ```

      ## [1] 67

- heartrate_seconds_merged.csv

  ``` r
  HR <- read_csv(here("/cloud/project/fitbit_data/HR.csv")) 

    str(HR)
  ```

      ## spc_tbl_ [2,483,658 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id   : num [1:2483658] 2.02e+09 2.02e+09 2.02e+09 2.02e+09 2.02e+09 ...
      ##  $ Time : chr [1:2483658] "4/12/2016 7:21:00 AM" "4/12/2016 7:21:05 AM" "4/12/2016 7:21:10 AM" "4/12/2016 7:21:20 AM" ...
      ##  $ Value: num [1:2483658] 97 102 105 103 101 95 91 93 94 93 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   Time = col_character(),
      ##   ..   Value = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
    colnames(HR)
  ```

      ## [1] "Id"    "Time"  "Value"

  ``` r
    head(HR)
  ```

      ## # A tibble: 6 × 3
      ##           Id Time                 Value
      ##        <dbl> <chr>                <dbl>
      ## 1 2022484408 4/12/2016 7:21:00 AM    97
      ## 2 2022484408 4/12/2016 7:21:05 AM   102
      ## 3 2022484408 4/12/2016 7:21:10 AM   105
      ## 4 2022484408 4/12/2016 7:21:20 AM   103
      ## 5 2022484408 4/12/2016 7:21:25 AM   101
      ## 6 2022484408 4/12/2016 7:22:05 AM    95

  ``` r
    nrow(HR)
  ```

      ## [1] 2483658

- dailyIntensities_merged.csv

  ``` r
  daily_intensities <- read_csv(here("/cloud/project/fitbit_data/daily_intensities.csv"))

    str(daily_intensities)
  ```

      ## spc_tbl_ [940 × 10] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id                      : num [1:940] 1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
      ##  $ ActivityDay             : chr [1:940] "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
      ##  $ SedentaryMinutes        : num [1:940] 728 776 1218 726 773 ...
      ##  $ LightlyActiveMinutes    : num [1:940] 328 217 181 209 221 164 233 264 205 211 ...
      ##  $ FairlyActiveMinutes     : num [1:940] 13 19 11 34 10 20 16 31 12 8 ...
      ##  $ VeryActiveMinutes       : num [1:940] 25 21 30 29 36 38 42 50 28 19 ...
      ##  $ SedentaryActiveDistance : num [1:940] 0 0 0 0 0 0 0 0 0 0 ...
      ##  $ LightActiveDistance     : num [1:940] 6.06 4.71 3.91 2.83 5.04 ...
      ##  $ ModeratelyActiveDistance: num [1:940] 0.55 0.69 0.4 1.26 0.41 ...
      ##  $ VeryActiveDistance      : num [1:940] 1.88 1.57 2.44 2.14 2.71 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   ActivityDay = col_character(),
      ##   ..   SedentaryMinutes = col_double(),
      ##   ..   LightlyActiveMinutes = col_double(),
      ##   ..   FairlyActiveMinutes = col_double(),
      ##   ..   VeryActiveMinutes = col_double(),
      ##   ..   SedentaryActiveDistance = col_double(),
      ##   ..   LightActiveDistance = col_double(),
      ##   ..   ModeratelyActiveDistance = col_double(),
      ##   ..   VeryActiveDistance = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
    colnames(daily_intensities)
  ```

      ##  [1] "Id"                       "ActivityDay"             
      ##  [3] "SedentaryMinutes"         "LightlyActiveMinutes"    
      ##  [5] "FairlyActiveMinutes"      "VeryActiveMinutes"       
      ##  [7] "SedentaryActiveDistance"  "LightActiveDistance"     
      ##  [9] "ModeratelyActiveDistance" "VeryActiveDistance"

  ``` r
    head(daily_intensities)
  ```

      ## # A tibble: 6 × 10
      ##         Id ActivityDay SedentaryMinutes LightlyActiveMinutes FairlyActiveMinutes
      ##      <dbl> <chr>                  <dbl>                <dbl>               <dbl>
      ## 1   1.50e9 4/12/2016                728                  328                  13
      ## 2   1.50e9 4/13/2016                776                  217                  19
      ## 3   1.50e9 4/14/2016               1218                  181                  11
      ## 4   1.50e9 4/15/2016                726                  209                  34
      ## 5   1.50e9 4/16/2016                773                  221                  10
      ## 6   1.50e9 4/17/2016                539                  164                  20
      ## # ℹ 5 more variables: VeryActiveMinutes <dbl>, SedentaryActiveDistance <dbl>,
      ## #   LightActiveDistance <dbl>, ModeratelyActiveDistance <dbl>,
      ## #   VeryActiveDistance <dbl>

  ``` r
    nrow(daily_intensities)
  ```

      ## [1] 940

- dailyCalories_merged.csv

  ``` r
  daily_calories  <- read_csv(here("/cloud/project/fitbit_data/daily_calories.csv"))

    str(daily_calories)
  ```

      ## spc_tbl_ [940 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
      ##  $ Id         : num [1:940] 8.25e+09 6.29e+09 1.50e+09 8.58e+09 3.98e+09 ...
      ##  $ ActivityDay: chr [1:940] "12/9/2017" "9/16/2018" "10/18/2018" "11/6/2018" ...
      ##  $ Calories   : num [1:940] 0 0 0 0 52 57 120 257 403 665 ...
      ##  - attr(*, "spec")=
      ##   .. cols(
      ##   ..   Id = col_double(),
      ##   ..   ActivityDay = col_character(),
      ##   ..   Calories = col_double()
      ##   .. )
      ##  - attr(*, "problems")=<externalptr>

  ``` r
    colnames(daily_calories)
  ```

      ## [1] "Id"          "ActivityDay" "Calories"

  ``` r
    head(daily_calories)
  ```

      ## # A tibble: 6 × 3
      ##           Id ActivityDay Calories
      ##        <dbl> <chr>          <dbl>
      ## 1 8253242879 12/9/2017          0
      ## 2 6290855005 9/16/2018          0
      ## 3 1503960366 10/18/2018         0
      ## 4 8583815059 11/6/2018          0
      ## 5 3977333714 10/3/2018         52
      ## 6 8792009665 9/22/2018         57

  ``` r
    nrow(daily_calories)
  ```

      ## [1] 940

### Cleaning (Process)

#### <span style="color: darkblue;">Reviewing</span>

For all data sets I used glimpse() and skim_without_charts to quickly
review the data.

``` r
# glimpse
glimpse(daily_activity)
```

    ## Rows: 940
    ## Columns: 15
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ date                     <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-…
    ## $ TotalSteps               <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
    ## $ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveMinutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ FairlyActiveMinutes      <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ LightlyActiveMinutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ SedentaryMinutes         <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ Calories                 <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…

``` r
glimpse(sleep_day)
```

    ## Rows: 413
    ## Columns: 5
    ## $ Id                 <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150…
    ## $ SleepDay           <chr> "4/12/2016 12:00:00 AM", "4/13/2016 12:00:00 AM", "…
    ## $ TotalSleepRecords  <dbl> 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ TotalMinutesAsleep <dbl> 327, 384, 412, 340, 700, 304, 360, 325, 361, 430, 2…
    ## $ TotalTimeInBed     <dbl> 346, 407, 442, 367, 712, 320, 377, 364, 384, 449, 3…

``` r
glimpse(weight)
```

    ## Rows: 67
    ## Columns: 8
    ## $ Id             <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 2873212…
    ## $ Date           <chr> "5/2/2016 11:59:59 PM", "5/3/2016 11:59:59 PM", "4/13/2…
    ## $ WeightKg       <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70.3, …
    ## $ WeightPounds   <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 159.6…
    ## $ Fat            <dbl> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ BMI            <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27.25,…
    ## $ IsManualReport <lgl> TRUE, TRUE, FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    ## $ LogId          <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e+12,…

``` r
  # glimpse(HR)
glimpse(daily_intensities)
```

    ## Rows: 940
    ## Columns: 10
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityDay              <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/…
    ## $ SedentaryMinutes         <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ LightlyActiveMinutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ FairlyActiveMinutes      <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ VeryActiveMinutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…

``` r
glimpse(daily_calories)
```

    ## Rows: 940
    ## Columns: 3
    ## $ Id          <dbl> 8253242879, 6290855005, 1503960366, 8583815059, 3977333714…
    ## $ ActivityDay <chr> "12/9/2017", "9/16/2018", "10/18/2018", "11/6/2018", "10/3…
    ## $ Calories    <dbl> 0, 0, 0, 0, 52, 57, 120, 257, 403, 665, 741, 928, 1002, 10…

``` r
# skim without charts
skim_without_charts(daily_activity)
```

|                                                  |                |
|:-------------------------------------------------|:---------------|
| Name                                             | daily_activity |
| Number of rows                                   | 940            |
| Number of columns                                | 15             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                |
| Column type frequency:                           |                |
| Date                                             | 1              |
| numeric                                          | 14             |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                |
| Group variables                                  | None           |

Data summary

**Variable type: Date**

| skim_variable | n_missing | complete_rate | min        | max        | median     | n_unique |
|:--------------|----------:|--------------:|:-----------|:-----------|:-----------|---------:|
| date          |         0 |             1 | 2016-04-12 | 2016-05-12 | 2016-04-26 |       31 |

**Variable type: numeric**

| skim_variable            | n_missing | complete_rate |         mean |           sd |         p0 |          p25 |          p50 |          p75 |         p100 |
|:-------------------------|----------:|--------------:|-------------:|-------------:|-----------:|-------------:|-------------:|-------------:|-------------:|
| Id                       |         0 |             1 | 4.855407e+09 | 2.424805e+09 | 1503960366 | 2.320127e+09 | 4.445115e+09 | 6.962181e+09 | 8.877689e+09 |
| TotalSteps               |         0 |             1 | 7.637910e+03 | 5.087150e+03 |          0 | 3.789750e+03 | 7.405500e+03 | 1.072700e+04 | 3.601900e+04 |
| TotalDistance            |         0 |             1 | 5.490000e+00 | 3.920000e+00 |          0 | 2.620000e+00 | 5.240000e+00 | 7.710000e+00 | 2.803000e+01 |
| TrackerDistance          |         0 |             1 | 5.480000e+00 | 3.910000e+00 |          0 | 2.620000e+00 | 5.240000e+00 | 7.710000e+00 | 2.803000e+01 |
| LoggedActivitiesDistance |         0 |             1 | 1.100000e-01 | 6.200000e-01 |          0 | 0.000000e+00 | 0.000000e+00 | 0.000000e+00 | 4.940000e+00 |
| VeryActiveDistance       |         0 |             1 | 1.500000e+00 | 2.660000e+00 |          0 | 0.000000e+00 | 2.100000e-01 | 2.050000e+00 | 2.192000e+01 |
| ModeratelyActiveDistance |         0 |             1 | 5.700000e-01 | 8.800000e-01 |          0 | 0.000000e+00 | 2.400000e-01 | 8.000000e-01 | 6.480000e+00 |
| LightActiveDistance      |         0 |             1 | 3.340000e+00 | 2.040000e+00 |          0 | 1.950000e+00 | 3.360000e+00 | 4.780000e+00 | 1.071000e+01 |
| SedentaryActiveDistance  |         0 |             1 | 0.000000e+00 | 1.000000e-02 |          0 | 0.000000e+00 | 0.000000e+00 | 0.000000e+00 | 1.100000e-01 |
| VeryActiveMinutes        |         0 |             1 | 2.116000e+01 | 3.284000e+01 |          0 | 0.000000e+00 | 4.000000e+00 | 3.200000e+01 | 2.100000e+02 |
| FairlyActiveMinutes      |         0 |             1 | 1.356000e+01 | 1.999000e+01 |          0 | 0.000000e+00 | 6.000000e+00 | 1.900000e+01 | 1.430000e+02 |
| LightlyActiveMinutes     |         0 |             1 | 1.928100e+02 | 1.091700e+02 |          0 | 1.270000e+02 | 1.990000e+02 | 2.640000e+02 | 5.180000e+02 |
| SedentaryMinutes         |         0 |             1 | 9.912100e+02 | 3.012700e+02 |          0 | 7.297500e+02 | 1.057500e+03 | 1.229500e+03 | 1.440000e+03 |
| Calories                 |         0 |             1 | 2.303610e+03 | 7.181700e+02 |          0 | 1.828500e+03 | 2.134000e+03 | 2.793250e+03 | 4.900000e+03 |

``` r
skim_without_charts(sleep_day)
```

|                                                  |           |
|:-------------------------------------------------|:----------|
| Name                                             | sleep_day |
| Number of rows                                   | 413       |
| Number of columns                                | 5         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |           |
| Column type frequency:                           |           |
| character                                        | 1         |
| numeric                                          | 4         |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |           |
| Group variables                                  | None      |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| SleepDay      |         0 |             1 |  20 |  21 |     0 |       31 |          0 |

**Variable type: numeric**

| skim_variable      | n_missing | complete_rate |         mean |          sd |         p0 |        p25 |        p50 |        p75 |       p100 |
|:-------------------|----------:|--------------:|-------------:|------------:|-----------:|-----------:|-----------:|-----------:|-----------:|
| Id                 |         0 |             1 | 5.000979e+09 | 2.06036e+09 | 1503960366 | 3977333714 | 4702921684 | 6962181067 | 8792009665 |
| TotalSleepRecords  |         0 |             1 | 1.120000e+00 | 3.50000e-01 |          1 |          1 |          1 |          1 |          3 |
| TotalMinutesAsleep |         0 |             1 | 4.194700e+02 | 1.18340e+02 |         58 |        361 |        433 |        490 |        796 |
| TotalTimeInBed     |         0 |             1 | 4.586400e+02 | 1.27100e+02 |         61 |        403 |        463 |        526 |        961 |

``` r
skim_without_charts(weight)
```

|                                                  |        |
|:-------------------------------------------------|:-------|
| Name                                             | weight |
| Number of rows                                   | 67     |
| Number of columns                                | 8      |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |        |
| Column type frequency:                           |        |
| character                                        | 1      |
| logical                                          | 1      |
| numeric                                          | 6      |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |        |
| Group variables                                  | None   |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| Date          |         0 |             1 |  19 |  21 |     0 |       56 |          0 |

**Variable type: logical**

| skim_variable  | n_missing | complete_rate | mean | count            |
|:---------------|----------:|--------------:|-----:|:-----------------|
| IsManualReport |         0 |             1 | 0.61 | TRU: 41, FAL: 26 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |         mean |           sd |           p0 |          p25 |          p50 |          p75 |         p100 |
|:--------------|----------:|--------------:|-------------:|-------------:|-------------:|-------------:|-------------:|-------------:|-------------:|
| Id            |         0 |          1.00 | 7.009282e+09 | 1.950322e+09 | 1.503960e+09 | 6.962181e+09 | 6.962181e+09 | 8.877689e+09 | 8.877689e+09 |
| WeightKg      |         0 |          1.00 | 7.204000e+01 | 1.392000e+01 | 5.260000e+01 | 6.140000e+01 | 6.250000e+01 | 8.505000e+01 | 1.335000e+02 |
| WeightPounds  |         0 |          1.00 | 1.588100e+02 | 3.070000e+01 | 1.159600e+02 | 1.353600e+02 | 1.377900e+02 | 1.875000e+02 | 2.943200e+02 |
| Fat           |        65 |          0.03 | 2.350000e+01 | 2.120000e+00 | 2.200000e+01 | 2.275000e+01 | 2.350000e+01 | 2.425000e+01 | 2.500000e+01 |
| BMI           |         0 |          1.00 | 2.519000e+01 | 3.070000e+00 | 2.145000e+01 | 2.396000e+01 | 2.439000e+01 | 2.556000e+01 | 4.754000e+01 |
| LogId         |         0 |          1.00 | 1.461772e+12 | 7.829948e+08 | 1.460444e+12 | 1.461079e+12 | 1.461802e+12 | 1.462375e+12 | 1.463098e+12 |

``` r
  # skim_without_charts(HR)
skim_without_charts(daily_intensities)
```

|                                                  |                   |
|:-------------------------------------------------|:------------------|
| Name                                             | daily_intensities |
| Number of rows                                   | 940               |
| Number of columns                                | 10                |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                   |
| Column type frequency:                           |                   |
| character                                        | 1                 |
| numeric                                          | 9                 |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                   |
| Group variables                                  | None              |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| ActivityDay   |         0 |             1 |   8 |   9 |     0 |       31 |          0 |

**Variable type: numeric**

| skim_variable            | n_missing | complete_rate |         mean |           sd |         p0 |          p25 |          p50 |          p75 |         p100 |
|:-------------------------|----------:|--------------:|-------------:|-------------:|-----------:|-------------:|-------------:|-------------:|-------------:|
| Id                       |         0 |             1 | 4.855407e+09 | 2.424805e+09 | 1503960366 | 2.320127e+09 | 4.445115e+09 | 6.962181e+09 | 8.877689e+09 |
| SedentaryMinutes         |         0 |             1 | 9.912100e+02 | 3.012700e+02 |          0 | 7.297500e+02 | 1.057500e+03 | 1.229500e+03 | 1.440000e+03 |
| LightlyActiveMinutes     |         0 |             1 | 1.928100e+02 | 1.091700e+02 |          0 | 1.270000e+02 | 1.990000e+02 | 2.640000e+02 | 5.180000e+02 |
| FairlyActiveMinutes      |         0 |             1 | 1.356000e+01 | 1.999000e+01 |          0 | 0.000000e+00 | 6.000000e+00 | 1.900000e+01 | 1.430000e+02 |
| VeryActiveMinutes        |         0 |             1 | 2.116000e+01 | 3.284000e+01 |          0 | 0.000000e+00 | 4.000000e+00 | 3.200000e+01 | 2.100000e+02 |
| SedentaryActiveDistance  |         0 |             1 | 0.000000e+00 | 1.000000e-02 |          0 | 0.000000e+00 | 0.000000e+00 | 0.000000e+00 | 1.100000e-01 |
| LightActiveDistance      |         0 |             1 | 3.340000e+00 | 2.040000e+00 |          0 | 1.950000e+00 | 3.360000e+00 | 4.780000e+00 | 1.071000e+01 |
| ModeratelyActiveDistance |         0 |             1 | 5.700000e-01 | 8.800000e-01 |          0 | 0.000000e+00 | 2.400000e-01 | 8.000000e-01 | 6.480000e+00 |
| VeryActiveDistance       |         0 |             1 | 1.500000e+00 | 2.660000e+00 |          0 | 0.000000e+00 | 2.100000e-01 | 2.050000e+00 | 2.192000e+01 |

``` r
skim_without_charts(daily_calories)
```

|                                                  |                |
|:-------------------------------------------------|:---------------|
| Name                                             | daily_calories |
| Number of rows                                   | 940            |
| Number of columns                                | 3              |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |                |
| Column type frequency:                           |                |
| character                                        | 1              |
| numeric                                          | 2              |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |                |
| Group variables                                  | None           |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| ActivityDay   |         0 |             1 |   8 |  10 |     0 |      940 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |         mean |           sd |         p0 |          p25 |        p50 |          p75 |       p100 |
|:--------------|----------:|--------------:|-------------:|-------------:|-----------:|-------------:|-----------:|-------------:|-----------:|
| Id            |         0 |             1 | 4.855407e+09 | 2.424805e+09 | 1503960366 | 2320127002.0 | 4445114986 | 6.962181e+09 | 8877689391 |
| Calories      |         0 |             1 | 2.303610e+03 | 7.181700e+02 |          0 |       1828.5 |       2134 | 2.793250e+03 |       4900 |

#### <span style="color: darkblue;">Error Correction</span>

Then, I checked the data for errors (missing values, duplicates).

- daily_activity, daily_intensities, & daily_calories: No errors found

  ``` r
  # daily activity 
    # locate & remove duplicates 
    sum(duplicated(daily_activity))
  ```

      ## [1] 0

  ``` r
    # locate missing values
    sum(is.na(daily_activity))
  ```

      ## [1] 0

  ``` r
  # daily intensities
    # locate & remove duplicates 
    sum(duplicated(daily_intensities))
  ```

      ## [1] 0

  ``` r
    # locate missing values
    sum(is.na(daily_intensities))
  ```

      ## [1] 0

  ``` r
  # daily calories 
    # locate & remove duplicates 
    sum(duplicated(daily_calories))
  ```

      ## [1] 0

  ``` r
    # locate missing values
    sum(is.na(daily_calories))
  ```

      ## [1] 0

- sleep_day: 3 duplicates found & removed

  ``` r
  # locate & remove duplicates 
  sum(duplicated(sleep_day))
  ```

      ## [1] 3

  ``` r
  sleep_day %>% distinct()
  ```

      ## # A tibble: 410 × 5
      ##            Id SleepDay       TotalSleepRecords TotalMinutesAsleep TotalTimeInBed
      ##         <dbl> <chr>                      <dbl>              <dbl>          <dbl>
      ##  1 1503960366 4/12/2016 12:…                 1                327            346
      ##  2 1503960366 4/13/2016 12:…                 2                384            407
      ##  3 1503960366 4/15/2016 12:…                 1                412            442
      ##  4 1503960366 4/16/2016 12:…                 2                340            367
      ##  5 1503960366 4/17/2016 12:…                 1                700            712
      ##  6 1503960366 4/19/2016 12:…                 1                304            320
      ##  7 1503960366 4/20/2016 12:…                 1                360            377
      ##  8 1503960366 4/21/2016 12:…                 1                325            364
      ##  9 1503960366 4/23/2016 12:…                 1                361            384
      ## 10 1503960366 4/24/2016 12:…                 1                430            449
      ## # ℹ 400 more rows

  ``` r
  # locate missing values
  sum(is.na(sleep_day))
  ```

      ## [1] 0

- weight: The column ‘Fat’ contained 65 missing values so I removed it.

  ``` r
  # locate & remove duplicates 
  sum(duplicated(weight))
  ```

      ## [1] 0

  ``` r
  weight %>% distinct()
  ```

      ## # A tibble: 67 × 8
      ##            Id Date      WeightKg WeightPounds   Fat   BMI IsManualReport   LogId
      ##         <dbl> <chr>        <dbl>        <dbl> <dbl> <dbl> <lgl>            <dbl>
      ##  1 1503960366 5/2/2016…     52.6         116.    22  22.6 TRUE           1.46e12
      ##  2 1503960366 5/3/2016…     52.6         116.    NA  22.6 TRUE           1.46e12
      ##  3 1927972279 4/13/201…    134.          294.    NA  47.5 FALSE          1.46e12
      ##  4 2873212765 4/21/201…     56.7         125.    NA  21.5 TRUE           1.46e12
      ##  5 2873212765 5/12/201…     57.3         126.    NA  21.7 TRUE           1.46e12
      ##  6 4319703577 4/17/201…     72.4         160.    25  27.5 TRUE           1.46e12
      ##  7 4319703577 5/4/2016…     72.3         159.    NA  27.4 TRUE           1.46e12
      ##  8 4558609924 4/18/201…     69.7         154.    NA  27.2 TRUE           1.46e12
      ##  9 4558609924 4/25/201…     70.3         155.    NA  27.5 TRUE           1.46e12
      ## 10 4558609924 5/1/2016…     69.9         154.    NA  27.3 TRUE           1.46e12
      ## # ℹ 57 more rows

  ``` r
  # locate missing values
    # count
    sum(is.na(weight)) 
  ```

      ## [1] 65

  ``` r
      # 65 missing
    # count by column
    sapply(weight, function(x) sum(is.na(x)))
  ```

      ##             Id           Date       WeightKg   WeightPounds            Fat 
      ##              0              0              0              0             65 
      ##            BMI IsManualReport          LogId 
      ##              0              0              0

  ``` r
      # 65 missing from 'Fat'

  # remove column 'Fat'
  weight = subset(weight, select = -c(Fat))
  ```

- HR: I was unable to check for missing values and duplicates due to
  file size but have included the script below as text.

  ``` r
  # locate & remove duplicates 
    # sum(duplicated(weight))
    # weight %>% distinct()

  # locate missing values
    # sum(is.na(weight))
  ```

#### <span style="color: darkblue;">Formatting</span>

Next, I converted date and time columns (originally in character format)
in each data set to Date and verified correct formatting using str(). I
was unable to run the script for the heart rate data set due to its size
but have included it below as text.

<div>

``` r
# daily_activity
daily_activity <- read_csv(here("/cloud/project/fitbit_data/daily_activity.csv"),col_types = cols(ActivityDate = col_date(format = "%m/%d/%Y")))
  colnames(daily_activity)[2] <- "date"

# sleep_day
sleep_day$date <- mdy_hms(sleep_day$SleepDay) 
  sleep_day$date <- as.Date(sleep_day$date) 

# weight 
weight$date <- mdy_hms(weight$Date)
  weight$date <- as.Date(weight$date)

# daily_intensities [1st line: only worked for some? ]
daily_intensities$date=as.Date(daily_intensities$ActivityDay, format="%m/%d/%Y", tz=Sys.timezone())
  
# daily_calories [1st line: only worked for some?]
daily_calories$date=as.Date(daily_calories$ActivityDay, format="%m/%d/%Y", tz=Sys.timezone())

# HR (did not run)
 # HR$Time <- strsplit(HR$Time," AM")
   # HR$date <- mdy_hms(HR$Time) 
   # HR$date <- format(as.Date(HR$date,format="%m-%d-%Y %H:%M:%S"),format="%Y-%m-%d")
   # HR$date <- as.Date(HR$date)
```

</div>

### Analysis (Analyze)

#### <span style="color: darkblue;">Summarizing</span>

I assessed the total number of participants in each data set.

``` r
n_distinct(daily_activity$Id)
```

    ## [1] 33

``` r
n_distinct(sleep_day$Id)
```

    ## [1] 24

``` r
n_distinct(weight$Id)
```

    ## [1] 8

``` r
n_distinct(HR$Id)
```

    ## [1] 14

``` r
n_distinct(daily_intensities$Id)
```

    ## [1] 33

``` r
n_distinct(daily_calories$Id)
```

    ## [1] 33

- Typically, 30 is the lowest number of participants needed to draw
  statistical significance from a study. As we can see, there are only
  14 participants in the heart rate data set and 8 participants in the
  weight data set, so I will not be including them in my analysis.

- The daily activity, daily calories, and daily intensities day sets all
  included 33 participants and will be included. The sleep day data set
  only has 24 participants, but I have decided to include it as well
  because it is close to the cut-off. However, this is something to keep
  in mind when considering results of my analysis informed by the sleep
  day data set.

Next, I created some summary statistics for each data frame.

``` r
# daily activity 
daily_activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()
```

    ##    TotalSteps    TotalDistance    SedentaryMinutes    Calories   
    ##  Min.   :    0   Min.   : 0.000   Min.   :   0.0   Min.   :   0  
    ##  1st Qu.: 3790   1st Qu.: 2.620   1st Qu.: 729.8   1st Qu.:1828  
    ##  Median : 7406   Median : 5.245   Median :1057.5   Median :2134  
    ##  Mean   : 7638   Mean   : 5.490   Mean   : 991.2   Mean   :2304  
    ##  3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.:1229.5   3rd Qu.:2793  
    ##  Max.   :36019   Max.   :28.030   Max.   :1440.0   Max.   :4900

``` r
# sleep_day 
sleep_day %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()
```

    ##  TotalSleepRecords TotalMinutesAsleep TotalTimeInBed 
    ##  Min.   :1.000     Min.   : 58.0      Min.   : 61.0  
    ##  1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0  
    ##  Median :1.000     Median :433.0      Median :463.0  
    ##  Mean   :1.119     Mean   :419.5      Mean   :458.6  
    ##  3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0  
    ##  Max.   :3.000     Max.   :796.0      Max.   :961.0

``` r
# daily intensities 
daily_intensities %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes) %>%
  summary()
```

    ##  VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes
    ##  Min.   :  0.00    Min.   :  0.00      Min.   :  0.0        Min.   :   0.0  
    ##  1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0        1st Qu.: 729.8  
    ##  Median :  4.00    Median :  6.00      Median :199.0        Median :1057.5  
    ##  Mean   : 21.16    Mean   : 13.56      Mean   :192.8        Mean   : 991.2  
    ##  3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0        3rd Qu.:1229.5  
    ##  Max.   :210.00    Max.   :143.00      Max.   :518.0        Max.   :1440.0

``` r
# daily calories
daily_calories %>%
  select(Calories) %>%
  summary()
```

    ##     Calories   
    ##  Min.   :   0  
    ##  1st Qu.:1828  
    ##  Median :2134  
    ##  Mean   :2304  
    ##  3rd Qu.:2793  
    ##  Max.   :4900

#### <span style="color: darkblue;">Merging</span>

I merged the daily_activity and sleep_day data sets to streamline the
visualization process. Because sleep_day has less participants (24) than
daily_activity (33), I used and verified an outer join. This way all
participants from both daily_activity and sleep_day will be included.

``` r
merged_activity_sleep <- merge(daily_activity,sleep_day, by="Id", all = TRUE)
  n_distinct(merged_activity_sleep$Id)
```

    ## [1] 33

### Visualization & Key Findings (Share)

#### <span style="color: darkblue;">Relationship Between Minutes Asleep & Time In Bed</span>

``` r
ggplot(data=sleep_day,aes(x=TotalMinutesAsleep,y=TotalTimeInBed))+geom_point()+
  geom_smooth(method="loess",color="cornflowerblue")+
  theme_light()+
  labs(title= "Daily Minutes Asleep vs Time In Bed",
       x="Minutes Asleep",
       y="Minutes In Bed")+
  theme(plot.title = element_text(size=12),
        axis.title.x=element_text(size=10),
        axis.text.x=element_text(size=10),
        axis.title.y=element_text(size=10),
        axis.text.y=element_text(size=10))
```

![](bellabeat_case_study_files/figure-gfm/mins%20asleep%20x%20mins%20in%20bed-1.png)<!-- -->

Unsurprisingly, we see a nearly linear relationship between time in bed
and time asleep.

#### <span style="color: darkblue;">Relationship Between Daily Steps & Daily Calories</span>

``` r
ggplot(data=daily_activity,aes(x=TotalSteps,y=Calories))+geom_point()+
  geom_smooth(method="loess",color="cornflowerblue")+
  theme_light()+
  labs(title= "Daily Steps vs Calories",
       x="Daily Steps",
       y="Calories")+
  theme(plot.title = element_text(size=12),
        axis.title.x=element_text(size=10),
        axis.text.x=element_text(size=10),
        axis.title.y=element_text(size=10),
        axis.text.y=element_text(size=10))
```

![](bellabeat_case_study_files/figure-gfm/daily%20steps%20x%20cals-1.png)<!-- -->

We see a strong positive correlation between daily steps and daily
calories burned.

#### <span style="color: darkblue;">Relationship Between Daily Steps & Sedentary Minutes</span>

``` r
ggplot(data=daily_activity,aes(x=TotalSteps,y=SedentaryMinutes))+geom_point()+
  geom_smooth(method="loess",color="cornflowerblue")+
  theme_light()+
  labs(title= "Daily Steps vs Sedentary Minutes",
       x="Daily Steps",
       y="Sedentary Minutes")+
  theme(plot.title = element_text(size=12),
        axis.title.x=element_text(size=10),
        axis.text.x=element_text(size=10),
        axis.title.y=element_text(size=10),
        axis.text.y=element_text(size=10))
```

![](bellabeat_case_study_files/figure-gfm/daily%20steps%20x%20sedentary%20mins-1.png)<!-- -->

We see a negative correlation between daily steps and sedentary minutes
from 0 to about 15,000 steps, which is the range where the majority of
data points occur. From 15,000 to 30,000 steps we see a slight positive
correlation between fewer data points.

#### <span style="color: darkblue;">Activity Levels By Average Time Daily</span>

``` r
# calculate
  # avg intensity by day
  avg_daily_intensities <- daily_activity %>%
      group_by(date)%>%
      summarize(avg_very_active=mean(VeryActiveMinutes,na.rm=TRUE),
                avg_fairly_active=mean(FairlyActiveMinutes,na.rm=TRUE),
                avg_lightly_active=mean(LightlyActiveMinutes,na.rm=TRUE),
                avg_sedentary=mean(SedentaryMinutes,na.rm=TRUE)) %>%
      as.data.frame()
  # avg intensity daily
  n_distinct(daily_activity$date)
```

    ## [1] 31

``` r
  avg_intensities <- avg_daily_intensities %>%
      summarize(avg_very_active=mean(avg_very_active,na.rm=TRUE),
                avg_fairly_active=mean(avg_fairly_active,na.rm=TRUE),
                avg_lightly_active=mean(avg_lightly_active,na.rm=TRUE),
                avg_sedentary=mean(avg_sedentary,na.rm=TRUE)) %>%
      as.data.frame()
  
# transpose df
activity_level <- c("Very Active","Fairly Active","Lightly Active","Sedentary")
daily_average <- c(21,14,192,986)
final_avg_intensities <- data.frame(activity_level,daily_average)

# plot
ggplot(data=final_avg_intensities,aes(x=activity_level,y=daily_average))+geom_col(color="slategrey",fill="cornflowerblue")+
   theme_light()+
  labs(title="Average Daily Activity Levels", 
       x="Activity Level",
       y="Minutes")+
  theme(plot.title = element_text(size=12),
        axis.title.x=element_text(size=10),
        axis.text.x=element_text(size=10),
        axis.title.y=element_text(size=10,),
        axis.text.y=element_text(size=10))+
  geom_text(aes(label = daily_average), vjust = -0.3, size=3.5)
```

![](bellabeat_case_study_files/figure-gfm/intensities%20by%20time-1.png)<!-- -->

We see that on average, the vast majority of participants’ time was
spent sedentary (986 minutes or 16.4 hours per day), followed by lightly
active (192 minutes or 3.2 hours), then very active (21 minutes), and
finally fairly active (14 minutes).

#### <span style="color: darkblue;">Activity Levels By Percent</span>

``` r
# calculate 
    intensity_sums <- daily_activity %>%
      summarize(very_active_sum = sum(VeryActiveMinutes,na.rm=TRUE),
                fairly_active_sum = sum(FairlyActiveMinutes,na.rm=TRUE),
                lightly_active_sum = sum(LightlyActiveMinutes,na.rm=TRUE),
                sedentary_sum = sum(SedentaryMinutes,na.rm=TRUE))

# df 
  total <- c(19895,12751,181244,931738)
  Activity_Level <- c("Very Active (1.7%)", "Fairly Active (1.1%)", "Lightly Active (15.8%)", "Sedentary(81.3%)")
  intensities <- data.frame(Activity_Level, total)

# plot
  ggplot(intensities, aes(x="", y=total, fill=Activity_Level)) +
    geom_bar(stat="identity", width=1,color="white") +
    coord_polar("y", start=0)+
    theme_void()+ 
    labs(title="Activity Levels By Percent")+
    theme(plot.title = element_text(size=12, hjust=0))
```

![](bellabeat_case_study_files/figure-gfm/intensities%20by%20percent-1.png)<!-- -->

Here we see those same activity levels represented as percentages. Again
it is clear that the vast majority of participants’ time was spent
sedentary (81.3%) rather than active (18.7% total).

#### <span style="color: darkblue;">Key Findings: Summary</span>

All in all we discovered that time in bed was nearly linearly correlated
to time asleep, daily steps were positively correlated with daily
calories burned, daily steps and sedentary minutes were negatively
correlated between 0 and 15,000 steps (the range the majority of users
fell into), and that participants spent the majority of their time
sedentary (81.3%) rather than active (18.7%).

### Recommendations (Act)

1.  The data clearly shows that getting to bed is associated with
    getting sleep. Sleep is one of the metrics that Bellabeat’s product
    ‘Time’ already tracks, and it is essential for overall health. The
    [CDC](https://www.cdc.gov/sleep/about_sleep/how_much_sleep.html)
    recommends 7 or more hours of sleep per night for adults aged 18-60,
    7-9 hours for adults aged 61-64, and 7-8 hours for adults over 65.
    Additionally,
    [research](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4164903/)
    suggests that women need more sleep on average than men.

    - Because Bellabeat already markets primarily towards women, I
      suggest the marketing team highlighting the importance of proper
      sleep for women. I also suggest that they incorporate features to
      help users get to bed such as notifications, reminders, and
      streaks/badges for multiple nights over the recommended sleep
      threshold.

2.  The data shows that for the majority of users, more time spent
    sedentary per day correlates with less steps completed and vice
    versa. The data also shows that more steps completed per day
    correlates with more calories burned.

    - I suggest that the marketing team promote and reward meeting daily
      step recommendations (10,000 daily according to the
      [CDC](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.cdc.gov/diabetes/prevention/pdf/postcurriculum_session8.pdf))
      in order to stay active and burn calories. One way to encourage
      users could be incorporating a social component to daily steps
      metrics via the Bellabeat App, where users could connect with
      others to complete competitions and challenges.

3.  The data shows that on average users spend over 16 hours per day
    sedentary, just under 4 hours active, and of that 4 hours only 35
    minutes fairly or very active. These levels of activity are much
    lower than the “150 minutes of moderate-intensity aerobic physical
    activity or 75 minutes of vigorous-intensity physical activity”
    recommended by the
    [CDC](https://www.cdc.gov/physicalactivity/walking/index.htm#:~:text=The%20Physical%20Activity%20Guidelines%20for,an%20equivalent%20combination%20each%20week.).

    - I suggest that the marketing team promote the importance of
      moderate to intense physical activity by working with a public
      figure their target audience can connect with and look up to, such
      as a female Olympic athlete.

#### <span style="color: darkblue;">Things to Consider</span>

- The time frame of this study was 2 months, which is too short to draw
  conclusions about trends over time. However, I did review some
  variables and noticed, for example, a decrease in average steps per
  day between April and May. It would be interesting to explore these
  trends with a larger data set in the future.
- Bellabeat currently tracks calories but not weight or BMI. Although
  the sample size was not large enough to analyze the weight/BMI data
  from the Fitbit data set, I believe their inclusion of this data
  suggests a market for these functions, which Bellabeat may want to
  consider adding to their tracking device and/or app.

### Conclusion

Thank you for your interest in my Bellabeat case study! Any comments or
suggestions are welcome and appreciated. My name is Kirsten Goff and I
can be contacted at kirstenagoff@gmail.com. Please feel free to connect
with me on
[**Linkedin**](https://www.linkedin.com/in/kirsten-goff-research/) and
view more of my work in my
[**Portfolio**](https://github.com/kirstengoff/kirstengoff.github.io).
