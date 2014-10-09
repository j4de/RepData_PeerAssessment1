# Reproducible Research: Peer Assessment 1

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

## Introduction

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement -- a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

## Tasks
- Loading and preprocessing the data
- What is the mean total number of steps taken per day?
- What is the average daily activity pattern?
- Imputing missing values
- Are there differences in activity patterns between weekdays and weekends?

:

## Loading and preprocessing the data

```r
setwd("~/Github/RepData_PeerAssessment1")
## Load the activity.csv file into memory
act <- read.csv( "activity.csv", header=T)
## Transform the date from a Factor variable to a Date variable
act$date <- as.Date(act$date,"%Y-%m-%d")
```


## What is mean total number of steps taken per day?

```r
## remove 'na' rows from our dataset by subsetting
act_nonas <- act[!is.na(act$steps),]
## get total number to steps per day for histogram puposes
act_totsteps <- aggregate(steps ~ date, data=act_nonas, FUN="sum")
hist(act_totsteps$steps, xlab="Steps", main="Total number of steps taken each day")
```

![plot of chunk unnamed-chunk-2](./PA1_template_files/figure-html/unnamed-chunk-2.png) 

```r
## get mean number of steps per day
paste( "Mean number of steps per day:",mean(act_totsteps$steps))
```

```
## [1] "Mean number of steps per day: 10766.1886792453"
```

```r
## get median number of steps per day
paste( "Median number of steps per day:",median(act_totsteps$steps))
```

```
## [1] "Median number of steps per day: 10765"
```

## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?



You can also embed plots, for example:

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
