Bellabeat Case Study - A Google Data Analytics Capstone Project Using
Health Tracker Data
================
by Simona Cibulkova

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

## Background:

I have done this project as a part of the Google Data Analytics
Professional Certificate. The goal of this study was to gain an insight
into how consumers are using their smart devices and to make
recommendations for the marketing strategy of the company.

**The Bellabeat company** is a high-tech manufacturer that designs
innovative products that help women track their health and wellness. The
company believes it has the potential to become a global leader in the
smart device market, and analyzing smart device fitness data could
create new growth opportunities for the company.

The company offers the following products: **the Bellabeat app** for
tracking health data related to activity, sleep, stress, menstrual
cycle, and mindfulness; **the Leaf** wellness tracker that can be worn
as a bracelet, necklace, or clip, and syncs with the Bellabeat app to
track activity, sleep, and stress levels; **the Time** watch that tracks
activity, sleep, and stress levels and connects to the Bellabeat app for
health insights; **the Spring** water bottle that connects to the
Bellabeat app and tracks daily water intake to ensure people are
adequately hydrated; the Bellabeat membership that provides 24/7 access
to fully customized guidance on nutrition, activity, sleep, health,
beauty, and mindfulness based on lifestyle and goals.

#### Assignment:

As a guide to analyzing the data, I used the six steps of the data
analysis process (ask, prepare, process, analyze, share, and act).

#### Key Stakeholders:

-   **Urška Sršen**: Co-founder and Chief Creative Officer of Bellabeat
-   **Sando Mur**: Mathematician and co-founder of Bellabeat

#### Business Task:

Through the analysis of smart device usage data, we hope to gain insight
into consumer use of non-Bellabeat smart devices. The Bellabeat
marketing team would be given recommendations on how to use these trends
or insights to craft new strategies.

#### Business Objectives:

-   What are the trends identified?
-   How could these trends apply to Bellabeat customers?
-   How could these trends help influence Bellabeat marketing strategy?

#### Deliverables:

-   A clear description of the business task.
-   A description of all the data sources used.
-   A description of any cleaning or manipulation done to the raw
    datasets.
-   A summary of the results of the analysis.
-   The supporting visualizations and key results.

## Dataset

For this case study, I used the data provided by Mobius that are
available [online from Kaggle](https://www.kaggle.com/arashnic/fitbit).
Using this data does not require a license as it is public domain.

These datasets were generated via Amazon Mechanical Turk by respondents
to a distributed survey from 12th March to 12th May 2016. A total of 30
Health Tracker users agreed to submit personal tracker information,
including activity intensity, a number of steps, heart rate, calories
burned, and sleep monitoring. There are 18 .csv files containing data on
the various tracking metrics broken down by days, hours, or minutes.
Every data point has a timestamp and is assigned a user identifier.

#### Limitations of the Data

Data sources should be ROCCC, which stands for Reliable, Original,
Comprehensive, Current, and Cited.

##### **Reliable**

-   After inspecting the data set, it was discovered that there were 33
    unique user identifiers (not 30, as stated in the summary of the
    data set).
-   A sample size of 30 (33) may not accurately reflect the product
    usage of the greater population and is not representative of the
    entire fitness population.
-   Data belongs to Fitbit users whose consent was obtained to make
    their personal data available. Therefore, it is not a completely
    random sample.
-   Only data from the final month of the trial are included in the data
    set (April 12 - May 12).

##### **Original**

-   The data was collected via a survey, i.e., by a third party, making
    it difficult to assess its originality.

##### **Comprehensive**

-   Bellabeat tracked the actions of 33 participants over a period of 31
    days, but not all participants tracked the same activities, so there
    are many gaps in the information. Only 24 participants tracked their
    sleep data, and only 8 participants tracked their weight.
    Additionally, there are no variables such as age and height included
    in the data that can be used to evaluate the effectiveness of
    activity tracking.
-   It is not clear whether the distance is measured in miles or
    kilometers.

##### **Current**

-   The data was collected in 2016. Due to this, users daily activities,
    fitness and sleeping habits, diet, and food consumption may have
    changed since then, and thus the data may be out of date.

##### **Cited**

-   No available data.

Considering the small sample size and lack of demographic data, this
case study is more of a training exercise than a legitimate study of
actionable insights due to the high margin of error.

Consider this scenario: your company wants to market only to Czechs. The
Czech Republic is the only EU country where more than a third of people
aged 16 to 74 use smartwatches and other similar devices. That is the
equivalent of 2.8 million people. This is supported by data published by
[Eurostat last
year](https://ec.europa.eu/eurostat/web/products-eurostat-news/-/ddn-20210225-1?redirect=%2Feurostat%2F).
We would need a sample size of 385 to be able to perform an accurate
analysis (with a 95% confidence level and a 5% margin of error) of Czech
fitness tracker user behavior. [Link to Sample Size
Calculator](https://www.surveymonkey.com/mp/sample-size-calculator).
This analysis uses data from 33 users, giving us a margin of error of
18% for an analysis of the public, which is higher than the general
margin of error range of 4% - 8%. [Link to Margin of Error
Calculator](https://www.pollfish.com/margin-of-error-calculator).

While we may have enough samples for a smaller demographic, such as a
smaller area of population, we are unable to accurately tie the data to
a particular region/location since we do not have any demographic
information.

## Setting up an environment

In R, the required packages have been installed and uploaded.

``` r
# install.packages("tidyverse")
# install.packages("dplyr")
# install.packages("tidyr")
# install.packages("readr")
# install.packages("lubridate")
# install.packages("ggplot2")
# install.packages("tinytex")
# install.packages("ggpubr")
# install.packages("magrittr")
# install.packages("ggthemr")
# install.packages("psych")
# install.packages("gridExtra")
# install.packages("png")
# install.packages("grid")
# install.packages("hrbrthemes")
```

``` r
library(tidyverse)
library(dplyr)
library(tidyr)
library(readr)
library(lubridate)
library(ggplot2)
library(tinytex)
library(ggpubr)
library(magrittr)
library(ggthemr)
library(psych)
library(gridExtra)
library(hrbrthemes)
```

## Data selection

For further analysis, the following datasets were selected.

``` r
daily_activity <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/dailyActivity_merged.csv')

hourly_steps <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/hourlySteps_merged.csv')

daily_sleep <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/sleepDay_merged.csv')

hourly_calories <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/hourlyCalories_merged.csv')

hourly_intensity <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/hourlyIntensities_merged.csv')

weight <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/weightLogInfo_merged.csv')

heartrate <- read.csv('C:/Users/scibu/OneDrive/Plocha/Bellabeat_Project/data/heartrate_seconds_merged.csv')
```

## Data cleansing and transformation

The data were processed by cleaning and ensuring they are correct,
relevant, complete, free of errors, and not displaying any outliers by:

-   exploring and observing the data,
-   identifying missing values, processing them, and analyzing them,
-   formatting data - converting it into a specific format,
-   conducting preliminary statistical analyses.

## Examining the data and becoming familiar with it

To see the structure of the data, the data frames were initially
displayed as strings, tibbles or glimpses.

``` r
#str(hourly_steps)
str(daily_activity)
```

    ## 'data.frame':    940 obs. of  15 variables:
    ##  $ Id                      : num  1.5e+09 1.5e+09 1.5e+09 1.5e+09 1.5e+09 ...
    ##  $ ActivityDate            : chr  "4/12/2016" "4/13/2016" "4/14/2016" "4/15/2016" ...
    ##  $ TotalSteps              : int  13162 10735 10460 9762 12669 9705 13019 15506 10544 9819 ...
    ##  $ TotalDistance           : num  8.5 6.97 6.74 6.28 8.16 ...
    ##  $ TrackerDistance         : num  8.5 6.97 6.74 6.28 8.16 ...
    ##  $ LoggedActivitiesDistance: num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveDistance      : num  1.88 1.57 2.44 2.14 2.71 ...
    ##  $ ModeratelyActiveDistance: num  0.55 0.69 0.4 1.26 0.41 ...
    ##  $ LightActiveDistance     : num  6.06 4.71 3.91 2.83 5.04 ...
    ##  $ SedentaryActiveDistance : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ VeryActiveMinutes       : int  25 21 30 29 36 38 42 50 28 19 ...
    ##  $ FairlyActiveMinutes     : int  13 19 11 34 10 20 16 31 12 8 ...
    ##  $ LightlyActiveMinutes    : int  328 217 181 209 221 164 233 264 205 211 ...
    ##  $ SedentaryMinutes        : int  728 776 1218 726 773 539 1149 775 818 838 ...
    ##  $ Calories                : int  1985 1797 1776 1745 1863 1728 1921 2035 1786 1775 ...

``` r
#str(daily_sleep)
#str(hourly_calories)
#str(hourly_intensity)
#str(weight)
#str(heartrate)
```

``` r
#as_tibble(hourly_steps)
#as_tibble(daily_activity)
as_tibble(daily_sleep)
```

    ## # A tibble: 413 x 5
    ##            Id SleepDay          TotalSleepRecor~ TotalMinutesAsl~ TotalTimeInBed
    ##         <dbl> <chr>                        <int>            <int>          <int>
    ##  1 1503960366 4/12/2016 12:00:~                1              327            346
    ##  2 1503960366 4/13/2016 12:00:~                2              384            407
    ##  3 1503960366 4/15/2016 12:00:~                1              412            442
    ##  4 1503960366 4/16/2016 12:00:~                2              340            367
    ##  5 1503960366 4/17/2016 12:00:~                1              700            712
    ##  6 1503960366 4/19/2016 12:00:~                1              304            320
    ##  7 1503960366 4/20/2016 12:00:~                1              360            377
    ##  8 1503960366 4/21/2016 12:00:~                1              325            364
    ##  9 1503960366 4/23/2016 12:00:~                1              361            384
    ## 10 1503960366 4/24/2016 12:00:~                1              430            449
    ## # ... with 403 more rows

``` r
#as_tibble(hourly_calories)
#as_tibble(hourly_intensity)
#as_tibble(weight)
#as_tibble(heartrate)
```

``` r
#glimpse(hourly_steps)
#glimpse(daily_activity)
#glimpse(daily_sleep)
#glimpse(hourly_calories)
#glimpse(hourly_intensity)
glimpse(weight)
```

    ## Rows: 67
    ## Columns: 8
    ## $ Id             <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 2873212~
    ## $ Date           <chr> "5/2/2016 11:59:59 PM", "5/3/2016 11:59:59 PM", "4/13/2~
    ## $ WeightKg       <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70.3, ~
    ## $ WeightPounds   <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 159.6~
    ## $ Fat            <int> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA, NA,~
    ## $ BMI            <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27.25,~
    ## $ IsManualReport <chr> "True", "True", "False", "True", "True", "True", "True"~
    ## $ LogId          <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e+12,~

``` r
#glimpse(heartrate)
```

### Counting the number of users

Number of unique users in each dataset was counted.

``` r
n_distinct(daily_activity$Id)
```

    ## [1] 33

``` r
n_distinct(hourly_calories$Id)
```

    ## [1] 33

``` r
n_distinct(hourly_intensity$Id)
```

    ## [1] 33

``` r
n_distinct(hourly_steps$Id)
```

    ## [1] 33

``` r
n_distinct(daily_sleep$Id)
```

    ## [1] 24

``` r
n_distinct(weight$Id)
```

    ## [1] 8

``` r
n_distinct(heartrate$Id)
```

    ## [1] 14

##### Observations:

-   The following Daily and Hourly Activity data is available for the
    period of 2016-04-12 to 2016-05-12 with 33 unique IDs.
-   Although the dataset should have included 30 respondents, there are
    33 entries. It is possible that some respondents created additional
    identifiers while filling in the survey.
-   There are just 24 unique IDs in daily sleep data from 2016-04-12 to
    2016-05-12.
-   With 8 unique IDs, weight data is available from 2016-04-12 to
    2016-05-12.
-   With 14 unique identifiers, data on heart rate variability is
    available for the period from 2016-04-12 to 2016-05-12.
-   In other words, 73 percent of users are willing to track sleep time,
    only 24 percent are willing to track weight, and only 42% wish to
    track heart rate variability.
-   For selected features, users are checking at most their daily
    activity, hourly calories, hourly intensity, and hourly steps.

### Checking for NULL values

``` r
sum(is.na(daily_activity),is.na(hourly_calories),is.na(hourly_intensity),is.na(hourly_steps),is.na(daily_sleep),is.na(heartrate))
```

    ## [1] 0

``` r
sum(is.na(weight))
```

    ## [1] 65

``` r
which(is.na(weight))
```

    ##  [1] 270 271 272 273 275 276 277 278 279 280 281 282 283 284 285 286 287 288 289
    ## [20] 290 291 292 293 294 295 296 297 298 299 300 301 302 303 304 305 306 307 308
    ## [39] 309 310 311 312 313 314 315 316 317 318 319 320 321 322 323 324 325 326 327
    ## [58] 328 329 330 331 332 333 334 335

##### Observations:

Only the weight dataset contains null values. Identifying which column
has null values:

``` r
colnames(weight)[colSums(is.na(weight)) > 0]
```

    ## [1] "Fat"

``` r
weight <- select(weight, -Fat)
```

The only column that contained null values was Fat column, which was
then removed.

### Creating a Venn diagram

``` r
daily_ids <- unique(daily_activity$Id)
hourly_ids <- unique(hourly_calories$Id)
sleep_ids <- unique(daily_sleep$Id)
weight_ids <- unique(weight$Id)
heartrate_ids <- unique(heartrate$Id)


library(VennDiagram)
venn <- venn.diagram(x = list("Steps" = daily_ids,
                              "Sleep" = sleep_ids,
                              "Weight" = weight_ids,
                              "Heartrate" = heartrate_ids),
                     filename = "features_venn_diagram.png",
                     imagetype = "png",
                     fill = c("blue", "green", "yellow", "red"),
                     fontfamily = "sans",
                     fontface = "bold",
                     cat.fontface = "bold",
                     cat.fontfamily = "sans",
                     lwd = 3,
                     cex = 2
                     )
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

##### Observations:

Tracking steps is an essential feature, used by 100 percent of users.
With 72.7 percent of users, sleep tracking is the second most popular
feature. Less than half (42,4 percent) of users use heart rate tracking.
Fewer than one-quarter (24.2 percent) of users track weight. Just 9% of
users use all four tracking features simultaneously.

The following analyses will focus on tracking steps and sleep as these
are the most popular features.

### Data manipulation and transformation

Some data frames had to be formatted in YYYY-MM-DD so R could work well
with them. The date columns data types were then changed from character
to date time.

Activity Date is the date column but was found to be formatted as a
character type. It should be formatted as a data type for better
analysis.

A new column for a time of day was created for the hourly steps data
frame in order to analyze the data by time of day (i.e. 1:00 AM, 2:00
AM, etc.).

``` r
# formating original date+time column with am/pm
daily_sleep$Date <- parse_date_time(daily_sleep$SleepDay, '%m/%d/%Y %I:%M:%S %p')

# formating date from character type to date type
daily_activity$Date <- parse_date_time(daily_activity$ActivityDate, '%m/%d/%Y')

# formating date + time (in am/pm)
hourly_intensity$ActivityHour <- parse_date_time(hourly_intensity$ActivityHour, '%m/%d/%Y %I:%M:%S %p')
hourly_intensity$Date <- format(as.POSIXlt(hourly_intensity$ActivityHour), "%m/%d/%Y")
hourly_intensity$Time <- format(as.POSIXlt(hourly_intensity$ActivityHour), "%H:%M:%S")
hourly_intensity$Date <- parse_date_time(hourly_intensity$Date, '%m/%d/%Y')

# separating date+time, deleting time (which is irrelevant for the analysis) and formating date
weight <- weight %>% separate(Date, c("Date", "Time"), " ")
weight <- subset(weight, select=-Time)
weight$Date <- parse_date_time(weight$Date, '%m/%d/%Y')
```

For each observation, a column representing the day of the week
(e.g. Monday, Tuesday, etc.) was added.

``` r
daily_activity$Weekday <- format(as.Date(daily_activity$Date), "%A")
daily_activity$Weekday <- recode(daily_activity$Weekday, "pondělí" = "Monday","úterý" = 'Tuesday', "středa" = 'Wednesday', "čtvrtek" = "Thursday", "pátek" = "Friday", "sobota" = "Saturday", "neděle" = "Sunday")
daily_activity$Weekday <- ordered(daily_activity$Weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))

daily_sleep$Weekday <- format(as.Date(daily_sleep$Date), "%A")
daily_sleep$Weekday <- recode(daily_sleep$Weekday, "pondělí" = "Monday","úterý" = 'Tuesday', "středa" = 'Wednesday', "čtvrtek" = "Thursday", "pátek" = "Friday", "sobota" = "Saturday", "neděle" = "Sunday")
daily_sleep$Weekday <- ordered(daily_sleep$Weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"))
```

To record the total daily activity, a column was created for the total
minutes.

``` r
daily_activity$TotalMinutes <- rowSums(daily_activity [,11:14])
daily_activity$TotalActivityMinutes <- rowSums(daily_activity [,11:13])
```

Estimation of the amount of time the user spends asleep in bed:

``` r
daily_sleep <- mutate(daily_sleep,PercentOfSleep = (TotalMinutesAsleep/TotalTimeInBed)*100)
```

### Initial table summaries:

#### Basic statistics includes:

-   mean (average)
-   std (standard deviation)
-   min and max
-   25, 50 and 75 percentiles

#### *Weight summary*

``` r
weight %>%
  select(WeightKg,BMI) %>%
  summary()
```

    ##     WeightKg           BMI       
    ##  Min.   : 52.60   Min.   :21.45  
    ##  1st Qu.: 61.40   1st Qu.:23.96  
    ##  Median : 62.50   Median :24.39  
    ##  Mean   : 72.04   Mean   :25.19  
    ##  3rd Qu.: 85.05   3rd Qu.:25.56  
    ##  Max.   :133.50   Max.   :47.54

##### Observations:

The weight data of only eight participants has been logged. The average
daily weight log stands at 72.04 kg, and the average body mass index
stands at 25.19, which is at or near the bottom of the overweight range
of 25 to 29.9.

#### *Daily activity summary*

``` r
# explore total activity
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
# explore numbers of activity intensity
daily_activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()
```

    ##  VeryActiveMinutes FairlyActiveMinutes LightlyActiveMinutes
    ##  Min.   :  0.00    Min.   :  0.00      Min.   :  0.0       
    ##  1st Qu.:  0.00    1st Qu.:  0.00      1st Qu.:127.0       
    ##  Median :  4.00    Median :  6.00      Median :199.0       
    ##  Mean   : 21.16    Mean   : 13.56      Mean   :192.8       
    ##  3rd Qu.: 32.00    3rd Qu.: 19.00      3rd Qu.:264.0       
    ##  Max.   :210.00    Max.   :143.00      Max.   :518.0

##### Observations:

The summary shows that the average user is taking 7,638 steps a day,
which is less than the recommended 10,000 steps. According to the 3rd
quartile of TotalSteps stats, 75% of users maintain 10,000 steps per
day. Users get 21.16 minutes of very active or vigorous activity on
average per day, which translates into 148.12 minutes per week. The
average user meets the recommended 75 minutes of vigorous activity per
week. In order to counteract the effects of sitting up to 10 hours a
day, scientists recommend performing moderate to vigorous activity for
at least 40 minutes each day. In this study, participants spend an
average of 991.2 minutes sedentary, which equals 16.52 hours a day
(including roughly 7 hours of sleep). Such long periods of inactivity
can cause health problems. According to this summary, the average
individual burns 2304 calories a day. It takes 3,500 calories to lose
one pound of fat, so users intending to lose weight should burn more
than this.

#### *Average activities during different weekdays*

``` r
# merge datasets
daily_sleep <- merge(daily_activity, daily_sleep, by = c("Id", "Date", "Weekday"))

# check for number of unique ids (should be 24)
paste("Unique ids:", n_distinct(daily_sleep$Id))
```

    ## [1] "Unique ids: 24"

``` r
avg_daily_sleep_and_activity <- daily_sleep %>% 
  group_by(Weekday) %>% 
  summarize(avg_calories = mean(Calories),
            avg_steps = mean(TotalSteps),
            avg_distance = mean(TotalDistance),
            avg_running_min = mean(VeryActiveMinutes),
            avg_jogging_min = mean(FairlyActiveMinutes),
            avg_walking_min = mean(LightlyActiveMinutes),
            avg_sedentary_min = mean(SedentaryMinutes),
            avg_asleep_min = mean(TotalMinutesAsleep),
            avg_in_bed_min = mean(TotalTimeInBed),
            avg_awake_in_bed_min = mean(TotalTimeInBed-TotalMinutesAsleep),
            avg_sleep_sessions = mean(TotalSleepRecords))

as_tibble(avg_daily_sleep_and_activity)
```

    ## # A tibble: 7 x 12
    ##   Weekday   avg_calories avg_steps avg_distance avg_running_min avg_jogging_min
    ##   <ord>            <dbl>     <dbl>        <dbl>           <dbl>           <dbl>
    ## 1 Monday           2465.     9340.         6.61            32.6            19.0
    ## 2 Tuesday          2496.     9183.         6.43            30.6            20.0
    ## 3 Wednesday        2378.     8023.         5.72            21.3            16.7
    ## 4 Thursday         2316.     8205.         5.80            22.7            16.2
    ## 5 Friday           2330.     7901.         5.51            21.2            14.6
    ## 6 Saturday         2527.     9949.         7.10            27.2            23.1
    ## 7 Sunday           2277.     7298.         5.18            22.1            16.8
    ## # ... with 6 more variables: avg_walking_min <dbl>, avg_sedentary_min <dbl>,
    ## #   avg_asleep_min <dbl>, avg_in_bed_min <dbl>, avg_awake_in_bed_min <dbl>,
    ## #   avg_sleep_sessions <dbl>

##### Observations:

The above table shows that users are walking (light activity) at least
three hours a day, every day, during the week. A relatively high number
of minutes are spent sedentary each day.

#### *Sleep record summary*

``` r
# explore total sleep numbers
daily_sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed, PercentOfSleep) %>%
  summary()
```

    ##  TotalSleepRecords TotalMinutesAsleep TotalTimeInBed  PercentOfSleep  
    ##  Min.   :1.000     Min.   : 58.0      Min.   : 61.0   Min.   : 49.84  
    ##  1st Qu.:1.000     1st Qu.:361.0      1st Qu.:403.0   1st Qu.: 91.22  
    ##  Median :1.000     Median :433.0      Median :463.0   Median : 94.31  
    ##  Mean   :1.119     Mean   :419.5      Mean   :458.6   Mean   : 91.68  
    ##  3rd Qu.:1.000     3rd Qu.:490.0      3rd Qu.:526.0   3rd Qu.: 96.07  
    ##  Max.   :3.000     Max.   :796.0      Max.   :961.0   Max.   :100.00

##### Observations:

The average amount of time spent in bed is 458.6 minutes or 7.6 hours.

``` r
# Correlation plot with steps and sleep data
daily_sleep %>% 
  select(c(Calories, TotalSteps, TotalDistance, TotalMinutesAsleep, TotalTimeInBed, SedentaryMinutes, LightlyActiveMinutes, VeryActiveMinutes, FairlyActiveMinutes)) %>% 
  rename(Steps = TotalSteps, Km = TotalDistance, Sleep = TotalMinutesAsleep, Laying = TotalTimeInBed, Sit = SedentaryMinutes, Walking = LightlyActiveMinutes, Run = VeryActiveMinutes, Jogging = FairlyActiveMinutes) %>% 
  corPlot(cex = 1.1, show.legend = FALSE)
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-21-1.png)<!-- -->

##### Observations

There is a notable correlation between calories burned and time spent
running (0.61), whereas jogging (0.18) and walking (0.12), however, have
a much smaller correlation. As a result, running is a more effective
calorie-burning activity than walking or jogging.

There is a moderate negative correlation between sitting time and
walking time (-0.26). It has been found that the more a person sleeps,
the less time they spend sitting.

In agreement with expectations, distance and number of steps are highly
correlated (0.98). There are significant positive correlations between
these variables and calories burned, time spent walking, time spent
jogging, and time spent running. The amount of time spent sleeping seems
to have little influence (-0.19) on how many steps a person takes.

#### *Visualisation based on users activity*

``` r
group_by_usertype <- 
    daily_activity %>%
    summarise(
        user_type = factor(case_when(
        SedentaryMinutes > mean(SedentaryMinutes) & LightlyActiveMinutes < mean(LightlyActiveMinutes) & FairlyActiveMinutes < mean(FairlyActiveMinutes) & VeryActiveMinutes < mean(VeryActiveMinutes) ~ "Sedentary",
        SedentaryMinutes < mean(SedentaryMinutes) & LightlyActiveMinutes > mean(LightlyActiveMinutes) & FairlyActiveMinutes < mean(FairlyActiveMinutes) & VeryActiveMinutes < mean(VeryActiveMinutes) ~ "Lightly Active",
        SedentaryMinutes < mean(SedentaryMinutes) & LightlyActiveMinutes < mean(LightlyActiveMinutes) & FairlyActiveMinutes > mean(FairlyActiveMinutes) & VeryActiveMinutes < mean(VeryActiveMinutes) ~ "Fairly Active",
        SedentaryMinutes < mean(SedentaryMinutes) & LightlyActiveMinutes < mean(LightlyActiveMinutes) & FairlyActiveMinutes < mean(FairlyActiveMinutes) & VeryActiveMinutes > mean(VeryActiveMinutes) ~ "Very Active",
    ),levels=c("Sedentary", "Lightly Active", "Fairly Active", "Very Active")), Calories, Id=Id) %>%
    drop_na()

# plotting user categories
options(repr.plot.width = 10, repr.plot.height = 8)
ggthemr("light")
group_by_usertype %>%
group_by(user_type) %>%
summarise(total = n()) %>%
mutate(totals = sum(total)) %>%
group_by(user_type) %>%
summarise(total_percent = total / totals) %>%
ggplot(aes(user_type,y=total_percent, fill=user_type)) +
    geom_col()+
    scale_y_continuous(labels = scales::percent) +
    theme(legend.position="none") +
    labs(title="Users' Activity Distribution", y = "Percentage", x = "Type of Activity", caption = paste0("Data from 33 users from 4/12/2016 to 5/12/2016"))
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

##### Observations:

The graph illustrates that users spend most of their day in a sedentary
position. This is followed by light activities such as walking.
Movements that are very active (running) and fairly active (jogging) are
below 10%.

#### *Box plot of calories burned by user type*

``` r
options(repr.plot.width = 10, repr.plot.height = 8)
ggthemr("dust")
ggplot(group_by_usertype, aes(user_type, Calories, fill=user_type)) +
    geom_boxplot() +
    theme(legend.position="none") +
    labs(title="Calories Burned by User Type", x="User Type")
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

##### Observations:

Exercise intensity and calories burned are positively correlated, as can
be seen in the graph. Very active people (those who run) burn more
calories than moderately active ones (those who jog), lightly active
ones (those who walk), and sedentary ones (those who sit).

#### *Visualizing number of steps a day*

``` r
daily_activity_steps_grouped <- daily_activity %>%
  group_by(Weekday) %>%
  summarise(mean_total_steps = mean(TotalSteps))

head(daily_activity_steps_grouped)
```

    ## # A tibble: 6 x 2
    ##   Weekday   mean_total_steps
    ##   <ord>                <dbl>
    ## 1 Monday               7781.
    ## 2 Tuesday              8125.
    ## 3 Wednesday            7559.
    ## 4 Thursday             7406.
    ## 5 Friday               7448.
    ## 6 Saturday             8153.

``` r
options(repr.plot.width = 10, repr.plot.height = 8)
ggthemr("grass")
ggplot(daily_activity_steps_grouped, aes(x = Weekday, y = mean_total_steps, fill = mean_total_steps)) +
  geom_col() + 
  theme_bw()+
  labs(y = "Average Steps", 
      x = "Weekday", 
      title = "Average steps by week",
      fill = "TotalSteps")
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-24-1.png)<!-- -->

##### Observations:

On Tuesday and Saturday, users were most active, and on Sunday they were
least active.

#### *Plotting hourly intensity to see what time of day users were most active*

``` r
hour_mean <- hourly_intensity %>% 
  summarize(avg_total_intensity = mean(TotalIntensity))
hour_mean
```

    ##   avg_total_intensity
    ## 1            12.03534

``` r
options(repr.plot.width = 10, repr.plot.height = 8)
ggthemr('flat')
hourly_intensity %>%
  group_by(Time) %>%
  summarise(average_total_intensity = mean(TotalIntensity)) %>%
    ggplot(aes(Time, y=average_total_intensity, fill=average_total_intensity)) + 
        geom_histogram(stat = "identity") +
        theme_bw()+
        theme(axis.text.x = element_text(angle = 45)) +
        labs(title="Average Total Intensity During the Day",
            x="Time of the Day",
            y="Average Intensity")+
        annotate("text", x = 4, y = 13, label = "Mean hourly intensity: 12", color = 'black', size = 3)+
        geom_hline(yintercept = mean(hour_mean$avg_total_intensity),color="brown")
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

##### Observations:

There was a peak in activity around 7 PM with the most activity between
5 PM and 8 PM.

#### *Visualizing Calories and Total Steps*

Examining the relationship between Steps and calories burned.

``` r
ggthemr('lilac')
ggplot(data=daily_activity, aes(x=TotalSteps, y=Calories))+
  geom_point()+
  geom_smooth(method=lm)+
  labs(title = "Total Steps and Calories Burned", x="Total Steps", y="Calories")+
  scale_x_comma()
```

    ## `geom_smooth()` using formula 'y ~ x'

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

##### Observations:

The plot shows a positive correlation between total steps and calories
burned. There do, however, seem to be some outliers that do not appear
to follow the correlation. These include observations of 0 steps with
zero to minimal calories burned and an observation of more than 35,000
steps with fewer than 3,000 calories burned.

#### *Checking for a correlation between the different activities and the calories burnt*

``` r
ggthemr('light')
plot2 <- ggplot(data=daily_activity, aes(x=FairlyActiveMinutes, y=Calories))+
  geom_point()+
  geom_smooth()+
  labs(title="Calories Burned in Fairly Active Minutes", x= "Fairly Active Minutes")
plot1 <- ggplot(data=daily_activity, aes(x=LightlyActiveMinutes, y=Calories))+
  geom_point()+
  geom_smooth()+
  labs(title="Calories Burned in Lightly Active Minutes", x= "Lightly Active Minutes")

grid.arrange(arrangeGrob(plot1, plot2, ncol = 2),nrow = 1)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->

``` r
ggthemr('light')
ggplot(data=daily_activity, aes(x=VeryActiveMinutes, y=Calories))+
  geom_point()+
  geom_smooth()+
  labs(title="Calories Burned in Very Active Minutes", x= "Very Active Minutes")
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-28-1.png)<!-- -->

##### Observations

The intensity of exercise (walking, running, jogging) is associated with
the number of calories burned. Very active movement is the best
predictor of calories burned.

#### *Sleeping time during the week*

``` r
options(repr.plot.width = 8, repr.plot.height = 8)
ggthemr("light")
ggplot(data = avg_daily_sleep_and_activity)+ geom_col(mapping = aes(x = Weekday, y = avg_asleep_min, fill = avg_asleep_min))+
  labs(title = "Average Sleep eachDring the Week", 
       x="Week day", y = "Average Sleep Minutes", 
       fill = "Total Sleep Minutes")
```

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-29-1.png)<!-- -->

##### Observations:

On weekends, especially on Sunday, people tend to sleep more. During
working days, an average of 400 minutes (6-7 hours) is maintained.

#### *Total time in bed and total time of sleeping*

``` r
options(repr.plot.width = 8, repr.plot.height = 8)
ggthemr("flat")
ggplot(daily_sleep, aes(x=TotalMinutesAsleep, y=TotalTimeInBed)) + 
  geom_point()+
  geom_smooth()+
  labs(title="Time in Bed vs. Minutes Asleep", x="Total Time of Sleep", y="Total Time Spent in Bed")
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](The_Bellabeat_CaseStudy_files/figure-gfm/unnamed-chunk-30-1.png)<!-- -->

##### Observations:

Time asleep is positively related to total time spent in bed, but some
points are outliers, indicating that some people have trouble falling
asleep even after spending a long time in bed.

### Insights:

-   Users were most active on Saturdays and Tuesdays, and least active
    on Sundays. Over the course of the week, they were most active
    between 5 PM and 8 PM, with a peak around 7 PM (probably when and
    after getting home from work).

-   Calorie burnt rate varies with the type of exercise. Very active
    movement and distance traveled are the best predictors of calories
    burned. A positive correlation exists also between total steps and
    calories burned.

-   On average, users burned 2,303 calories and walked 7,637 steps per
    day. In terms of health benefits, that is almost 2400 steps fewer
    than the recommended 10,000 steps per day.

-   The average number of sedentary hours per day was 16.5 hours,
    including an average of 7 hours of sleep per day.

-   The majority of users kept track of sleep time - 73% did so.

-   The average body mass index recorded was 25.19, which is in the
    overweight category of 25 to 29.9. It suggests that most people are
    either not following a healthy diet or not exercising enough.
    However, only 24% of participants had logged their weight. The
    reasons for this need to be explored further. Are the weight logging
    procedures complicated? Are the participants not interested in
    tracking their weight progress?

-   The heart rate variability was tracked by only 42% of participants.
    A more thorough investigation is needed to discover why. Why aren
    not participants tracking their weight progress?

### Recommendations:

-   To conduct further analysis, more data should be collected. There is
    no demographic information in the current data. Hence, a larger
    sample size would be beneficial for conducting further analysis.

-   Educating and empowering users about fitness benefits (like a type
    of movement and calories burned) via the Bellabeat app could
    encourage them to exercise more. In addition, the alert
    notifications can motivate the user to reach the daily goal of
    10,000 steps.

-   Bellabeat can also provide notifications to encourage users to
    exercise on weekends or in the evening. I would suggest sending
    users a variety of reminders or strategies like suggesting they
    spread out their recommended 30 minutes of activity throughout the
    day and week and encouraging them to do some exercise if they have
    been sedentary for too long (for example, taking a walk every 2-3
    hours for 5-10 minutes or getting up and moving every 30 minutes to
    avoid risks associated with prolonged sitting and to boost
    productivity). The individual could be advised to perform 75 minutes
    of vigorous activity a week and to receive alert notifications.

-   Bellabeat could offer more than just steps, sleep time, weight, and
    heart rate tracking. It could offer sessions on eating habits,
    different types of exercises, etc.

-   The notifications can help users stay on track in their efforts to
    burn the necessary calories and reach their weight loss goals if
    they are trying to lose weight.

-   Bellabeat should make it easier to record weight and BMI. An example
    would be a wireless connection between a weighing machine and an
    app, or a built-in camera that allows customers to take a picture of
    their weight on a digital scale and enter it automatically into the
    log. Customers may be encouraged to weigh themselves by having a
    reminder icon or a beeping sound go off at selected times throughout
    the day.

-   Similarly to logging weight, new methods should be developed to make
    tracking sleep easier for customers. It is also important to
    emphasize the relationship between sleep quality and quantity and
    health and beauty.

-   For app users who want to improve their sleep time, Bellabeat could
    send a notification via the app prompting them to go to bed at a
    specific time.

-   Exercise with a friend is a great way to feel more motivated, to try
    new types of exercises, and to stay consistent. Consequently, social
    media workout apps could help users reach their goals by connecting
    them with friends and other users. By creating a supportive women’s
    group ready to prioritize their health, the Bellabeat app could
    become women’s go-to workout app. In such an environment, users
    could also post their favorite workouts, wellness tips, healthy
    recipes, etc. \|\|\|\|\|\| Users could add friends and view each
    other’s activity too. They could follow weekly fitness and wellness
    challenges.

-   Heartrate was not studied here so I would just recommend enabling
    alert notifications if their resting heart rate varied significantly
    from their normal level.
